FROM apache/airflow:3.3.2

ARG AIRFLOW_VERSION=3.3.2
ARG PYTHON_VERSION=3.13

USER root

# Copy Java 21 into the Airflow image
COPY --from=java /opt/java/openjdk /opt/java/openjdk

ENV JAVA_HOME=/opt/java/openjdk
ENV PATH="${JAVA_HOME}/bin:${PATH}"
ENV PYSPARK_PYTHON=python3
ENV PYSPARK_DRIVER_PYTHON=python3

USER airflow

RUN pip install --no-cache-dir \
    "apache-airflow==${AIRFLOW_VERSION}" \
    "apache-airflow-providers-openlineage" \
    "apache-airflow-providers-apache-spark" \
    "pyspark==3.5.7" \
    --constraint "https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"

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