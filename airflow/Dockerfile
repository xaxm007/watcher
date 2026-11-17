ARG AIRFLOW_VERSION=3.0.0
ARG PYTHON_VERSION=3.12

FROM apache/airflow:slim-${AIRFLOW_VERSION}-python${PYTHON_VERSION}

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
    --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-3.12.txt"

# Installed WITHOUT the constraint file: the constraints pin pyspark (3.5.5) and the
# openlineage provider (2.2.0, which breaks Spark parent-job injection on Airflow 3).
# pyspark must match the Spark cluster image (apache/spark:3.5.4).
ARG SPARK_VERSION=3.5.4
RUN pip install --no-cache-dir \
    "pyspark==${SPARK_VERSION}" \
    "apache-airflow-providers-openlineage>=2.10"


# # Get a Java 21 runtime
# FROM eclipse-temurin:21-jre AS java

# # Build the Airflow image
# FROM apache/airflow:3.3.2

# ARG AIRFLOW_VERSION=3.3.2
# ARG PYTHON_VERSION=3.13

# USER root

# # Copy Java 21 into the Airflow image
# COPY --from=java /opt/java/openjdk /opt/java/openjdk

# ENV JAVA_HOME=/opt/java/openjdk
# ENV PATH="${JAVA_HOME}/bin:${PATH}"
# ENV PYSPARK_PYTHON=python3
# ENV PYSPARK_DRIVER_PYTHON=python3

# USER airflow

# RUN pip install --no-cache-dir \
#     "apache-airflow==${AIRFLOW_VERSION}" \
#     "apache-airflow-providers-openlineage" \
#     "apache-airflow-providers-apache-spark" \
#     "pyspark==3.5.7" \
#     --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"






# FROM apache/airflow:3.3.2

# ARG AIRFLOW_VERSION=3.3.2
# ARG PYTHON_VERSION=3.13
# # ARG PYTHON_VERSION=3.13.15 # specific version

# USER root

# RUN apt-get update && \
#     apt-get install -y --no-install-recommends \
#         openjdk-21-jre-headless \
#         procps && \
#     rm -rf /var/lib/apt/lists/*

# ENV JAVA_HOME=/usr/lib/jvm/java-21-openjdk-arm64
# ENV PATH="${JAVA_HOME}/bin:${PATH}"

# ENV PYSPARK_PYTHON=python3
# ENV PYSPARK_DRIVER_PYTHON=python3

# USER airflow

# RUN pip install --no-cache-dir \
#     "apache-airflow==${AIRFLOW_VERSION}" \
#     "apache-airflow-providers-openlineage" \
#     "apache-airflow-providers-apache-spark" \
#     "pyspark==3.5.7" \
#     --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
# # RUN pip install --no-cache-dir \
# #     "apache-airflow==${AIRFLOW_VERSION}" \
# #     "apache-airflow-providers-openlineage" \
# #     --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"