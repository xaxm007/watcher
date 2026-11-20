ARG AIRFLOW_VERSION=3.0.0
ARG PYTHON_VERSION=3.12

FROM apache/airflow:slim-${AIRFLOW_VERSION}-python${PYTHON_VERSION}

# ARGs declared before FROM are not visible inside the stage; redeclare them.
ARG AIRFLOW_VERSION
ARG PYTHON_VERSION

USER root

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        openjdk-17-jre-headless \
        procps \
    && rm -rf /var/lib/apt/lists/* \
    && JAVA_HOME_PATH="$(dirname "$(dirname "$(readlink -f "$(command -v java)")")")" \
    && ln -s "${JAVA_HOME_PATH}" /usr/local/openjdk-17

ENV JAVA_HOME=/usr/local/openjdk-17
ENV PATH="${JAVA_HOME}/bin:${PATH}"

USER airflow

COPY requirements.txt /tmp/requirements.txt

RUN pip install --no-cache-dir \
    "apache-airflow==${AIRFLOW_VERSION}" \
    -r /tmp/requirements.txt \
    --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"

# Installed WITHOUT the constraint file: the constraints pin pyspark (3.5.5) and the
# openlineage provider (2.2.0, which breaks Spark parent-job injection on Airflow 3).
# pyspark must match the Spark cluster image (apache/spark:3.5.4). The "connect"
# extra adds the gRPC/pandas/pyarrow client deps for Spark Connect. pandas and
# pyarrow are pinned to the Airflow constraint versions: unpinned, pip pulls
# pandas 3 / pyarrow 26, and pyarrow 26 fails to import against Airflow's NumPy 1.26
# (pip check does not catch this).
ARG SPARK_VERSION=3.5.4
ARG OPENLINEAGE_PROVIDER_VERSION=2.21.0
RUN pip install --no-cache-dir \
    "pyspark[connect]==${SPARK_VERSION}" \
    "pandas==2.1.4" \
    "pyarrow==16.1.0" \
    "apache-airflow-providers-openlineage==${OPENLINEAGE_PROVIDER_VERSION}" \
    && pip check
