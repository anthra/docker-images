FROM anthra/ubuntu-base:18.04
MAINTAINER info@anthra.hu

ENV YOUTRACK_USER_ID=7000 \
    YOUTRACK_USER_NAME=youtrack \
    YOUTRACK_VERSION=2020.1 \
    YOUTRACK_BUILD=3588

# add our user and group first to make sure their IDs get assigned consistently, regardless of whatever dependencies get added
RUN \
    mkdir -p /var/lib/$YOUTRACK_USER_NAME && \
    groupadd --gid $YOUTRACK_USER_ID $YOUTRACK_USER_NAME && \
    useradd --system -d /var/lib/$YOUTRACK_USER_NAME --uid $YOUTRACK_USER_ID --gid $YOUTRACK_USER_NAME $YOUTRACK_USER_NAME && \
    chown -R $YOUTRACK_USER_NAME:$YOUTRACK_USER_NAME /var/lib/$YOUTRACK_USER_NAME

######### Install hub ###################
COPY entry-point.sh /entry-point.sh

RUN \
    export YOUTRACK_VERSION=$YOUTRACK_VERSION && \
    export YOUTRACK_BUILD=$YOUTRACK_BUILD && \
    mkdir -p /usr/local && \
    mkdir -p /var/lib/$YOUTRACK_USER_NAME && \
    cd /usr/local && \
    echo "$YOUTRACK_VERSION" > version.docker.image && \
    curl -L https://download.jetbrains.com/charisma/youtrack-${YOUTRACK_VERSION}.${YOUTRACK_BUILD}.zip > youtrack.zip && \
    unzip youtrack.zip && \
    mv /usr/local/youtrack-${YOUTRACK_VERSION}.${YOUTRACK_BUILD} /usr/local/$YOUTRACK_USER_NAME && \
    rm -f youtrack.zip && \
    rm -rf /usr/local/$YOUTRACK_USER_NAME/internal/java/linux-x64/man && \
    rm -rf /usr/local/$YOUTRACK_USER_NAME/internal/java/mac-x64 && \
    rm -rf /usr/local/$YOUTRACK_USER_NAME/internal/java/windows-amd64 && \
    chown -R $YOUTRACK_USER_NAME:$YOUTRACK_USER_NAME /usr/local/$YOUTRACK_USER_NAME && \
    chmod -R u+rwxX /usr/local/$YOUTRACK_USER_NAME/internal/java/linux-x64

USER $YOUTRACK_USER_NAME
ENV HOME=/var/lib/$YOUTRACK_USER_NAME
EXPOSE 8080
ENTRYPOINT ["/entry-point.sh"]
CMD ["run"]
