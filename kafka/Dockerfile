# Kafka and Zookeeper

FROM openjdk:8-jre-slim-buster

LABEL "org.opencontainers.image.source"="https://github.com/clavinjune/docker-kafka"

ENV DEBIAN_FRONTEND noninteractive
ENV SCALA_VERSION 2.13
ENV KAFKA_VERSION 3.3.1
ENV KAFKA_HOME /opt/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION"

# Install Kafka, Zookeeper and other needed things
RUN apt-get update
RUN apt-get install -y zookeeper wget supervisor dnsutils
RUN rm -rf /var/lib/apt/lists/*
RUN apt-get clean
RUN wget -q "https://dlcdn.apache.org/kafka/$KAFKA_VERSION/kafka_$SCALA_VERSION-$KAFKA_VERSION.tgz" \
    -O "/tmp/kafka_$SCALA_VERSION-$KAFKA_VERSION.tgz"
RUN tar zvxf "/tmp/kafka_$SCALA_VERSION-$KAFKA_VERSION.tgz" -C /opt
RUN rm "/tmp/kafka_$SCALA_VERSION-$KAFKA_VERSION.tgz"

ADD scripts/start-kafka.sh /usr/bin/start-kafka.sh

# Supervisor config
ADD supervisor/kafka.conf supervisor/zookeeper.conf /etc/supervisor/conf.d/

# 2181 is zookeeper, 9092 is kafka
EXPOSE 2181 9092

CMD ["supervisord", "-n"]
