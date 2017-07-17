# docker build -t app/mule --build-arg MULE_VERSION=example .
# docker run -d app/mule -name app-mule -link activemq:activemq
#                                           -p 63080:63080
#                                           -p 63081:63081
#                                           -p 63082:63082
#                                           -v /somewhere/where/apps-user:/opt/mule/apps
#                                           -v /somewhere/where/logs:/opt/mule/logs
#                                           -v /somewhere/where/conf-user:/opt/mule/conf
#                                           -v /somewhere/where/lib-user:/opt/mule/lib/user
#

# Pull base image
FROM java:7

# Mule ESB CE version number.
ARG MULE_VERSION=3.4.0

# Environment variables required for this build
ENV MULE=mule-standalone-$MULE_VERSION \
    MULE_HOME=/opt/mule \
    MULE_MQS=63080 MULE_WS=63081 MULE_PX=63082 \
    MULE_DOWNLOAD_URL=https://repository.mulesoft.org/nexus/content/repositories/releases/org/mule/distributions/mule-standalone/$MULE_VERSION/mule-standalone-$MULE_VERSION.tar.gz

# Environment path
ENV PATH=$PATH:$MULE_HOME:$MULE_HOME/bin

# Install Mule ESB.
RUN curl -O $MULE_DOWNLOAD_URL && \
    mkdir -p /opt && \
    tar xvzf $MULE.tar.gz -C /opt/ && \
    rm $MULE.tar.gz && \
    mv /opt/$MULE $MULE_HOME

# Set the owner of all Mule-related files to the user which will be used to run Mule.
RUN chmod a+xr $MULE_HOME && \
    useradd -b $MULE_HOME -m -s /bin/bash mule && \
    echo mule:mule | chpasswd && \
    chown mule:mule -R $MULE_HOME

# Define user
USER mule

# Define working directory.
WORKDIR $MULE_HOME

# Define mount points.
VOLUME ["$MULE_HOME/logs", "$MULE_HOME/conf", "$MULE_HOME/apps", "$MULE_HOME/domains" , "$MULE_HOME/lib/user"]

# Default http port
EXPOSE $MULE_MQS $MULE_WS $MULE_PX

# Default when starting the container is to start Mule ESB.
CMD ["mule"]