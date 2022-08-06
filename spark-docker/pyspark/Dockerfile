#
# Copyright 2018 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

ARG SPARK_IMAGE=apache/spark-py:v3.2.2
FROM ${SPARK_IMAGE}

# Switch to user root so we can add additional jars and configuration files.
USER root

# Setup dependencies for hadoop aws
ADD https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.3.1/hadoop-aws-3.3.1.jar $SPARK_HOME/jars
RUN chmod 644 $SPARK_HOME/jars/hadoop-aws-3.3.1.jar
ADD https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.11.901/aws-java-sdk-bundle-1.11.901.jar $SPARK_HOME/jars
RUN chmod 644 $SPARK_HOME/jars/aws-java-sdk-bundle-1.11.901.jar
ADD https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-common/3.3.1/hadoop-common-3.3.1.jar $SPARK_HOME/jars
RUN chmod 644 $SPARK_HOME/jars/hadoop-common-3.3.1.jar
ADD https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-client-core/3.3.1/hadoop-mapreduce-client-core-3.3.1.jar $SPARK_HOME/jars
RUN chmod 644 $SPARK_HOME/jars/hadoop-mapreduce-client-core-3.3.1.jar

# Setup dependencies for delta lake
ADD https://repo1.maven.org/maven2/io/delta/delta-core_2.12/2.0.0/delta-core_2.12-2.0.0.jar $SPARK_HOME/jars
RUN chmod 644 $SPARK_HOME/jars/delta-core_2.12-2.0.0.jar
ADD https://repo1.maven.org/maven2/io/delta/delta-storage/2.0.0/delta-storage-2.0.0.jar $SPARK_HOME/jars
RUN chmod 644 $SPARK_HOME/jars/delta-storage-2.0.0.jar

# Setup for the Prometheus JMX exporter.
# Add the Prometheus JMX exporter Java agent jar for exposing metrics sent to the JmxSink to Prometheus.
ADD https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.11.0/jmx_prometheus_javaagent-0.11.0.jar /prometheus/
RUN chmod 644 /prometheus/jmx_prometheus_javaagent-0.11.0.jar

USER ${spark_uid}

RUN mkdir -p /etc/metrics/conf
COPY conf/metrics.properties /etc/metrics/conf
COPY conf/prometheus.yaml /etc/metrics/conf

ENTRYPOINT ["/opt/entrypoint.sh"]
