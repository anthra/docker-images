FROM debian:stable-slim
LABEL MAINTAINER="anthra"

ARG userName="fah"
ARG uid="11111"
ARG gid="11111"
ARG version="v7.6"
ARG home="/opt/${userName}"
ARG clientDownloadUrl="https://download.foldingathome.org/releases/public/release/fahclient/debian-stable-64bit/${version}/latest.tar.bz2"

ENV USER "Anonymous"
ENV TEAM "0"
ENV ENABLE_GPU "false"
ENV ENABLE_SMP "true"
ENV POWER "light"
# "full", "medium", "light"


#LABEL maintainer="yurinnick" \
#      repository="https://github.com/yurinnick/folding-at-home-docker" \
#      description="Unofficial Folding@Home image for CPU compute" \
#      version="7.6"


# Update system and install tools
RUN \
    apt-get update -y && \
    apt-get install -y wget bzip2

# Add user
RUN \
    mkdir -p ${home} ${home}/work && \
    groupadd --gid ${gid} ${userName} && \
    useradd --system -d /opt/${userName} --uid ${uid} --gid ${gid} ${userName} && \
    chown -R ${userName}:${userName} ${home}

VOLUME ${home}/work

# Add entry point
COPY --chown=${userName}:${userName} fahclient.sh /
RUN chmod +x /fahclient.sh

# Download latest client version
RUN \
    wget ${clientDownloadUrl} -O /tmp/fahclient.tar.bz2 && \
    tar -xjf /tmp/fahclient.tar.bz2 -C ${home} --strip-components=1

# Fix permissions
RUN \
    chown -R ${userName}:${userName} ${home}

# Clean up
RUN rm -rf /tmp/fahclient.tar.bz2 && \
    apt-get purge -y wget bzip2 && \
    apt-get clean autoclean && \
    apt-get autoremove --yes && \
    rm -rf /var/lib/apt/lists/*

# Exposed ports
EXPOSE 7396
EXPOSE 36330

# Set User, Workdir, Entrypoint
USER ${userName}
WORKDIR ${home}
ENTRYPOINT /fahclient.sh

#ENTRYPOINT ["sleep", "1000"]
