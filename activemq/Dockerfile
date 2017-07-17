# docker build -t app/activemq --build-arg ACTIVEMQ_VERSION=example .
# docker run -d app/activemq -name app-activemq
#                              -p 61616:61616
#                              -p 8161:8161
#                              -v /somewhere/where/logs:/opt/activemq/data
#

# Pull base image
FROM java:7

# ActiveMQ version number.
ARG ACTIVEMQ_VERSION=5.13.3

# Environment variables required for this build
ENV ACTIVEMQ=apache-activemq-$ACTIVEMQ_VERSION \
    ACTIVEMQ_HOME=/opt/activemq \
    ACTIVEMQ_TCP=61616 ACTIVEMQ_AMQP=5672 ACTIVEMQ_STOMP=61613 ACTIVEMQ_MQTT=1883 ACTIVEMQ_WS=61614 ACTIVEMQ_UI=8161 \
    ACTIVEMQ_DOWNLOAD_URL=http://archive.apache.org/dist/activemq/$ACTIVEMQ_VERSION/apache-activemq-$ACTIVEMQ_VERSION-bin.tar.gz

RUN curl -O $ACTIVEMQ_DOWNLOAD_URL && \
    mkdir -p /opt && \
    tar xf $ACTIVEMQ-bin.tar.gz -C /opt/ && \
    rm $ACTIVEMQ-bin.tar.gz && \
    ln -s /opt/$ACTIVEMQ $ACTIVEMQ_HOME && \
    useradd -r -M -d $ACTIVEMQ_HOME activemq && \
    chown activemq:activemq /opt/$ACTIVEMQ -R

# Define user
USER activemq

# Define working directory.
WORKDIR $ACTIVEMQ_HOME

# Define mount points.
VOLUME ["$ACTIVEMQ_HOME/data"]

# Default http port
EXPOSE $ACTIVEMQ_TCP $ACTIVEMQ_AMQP $ACTIVEMQ_STOMP $ACTIVEMQ_MQTT $ACTIVEMQ_WS $ACTIVEMQ_UI

# Default when starting the container is to start ActiveMQ.
CMD ["/bin/bash", "-c", "bin/activemq console"]