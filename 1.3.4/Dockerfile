FROM resin/rpi-raspbian:jessie

# Dependencies
RUN apt-get update \
 && apt-get upgrade \
 && apt-get install locales wget ca-certificates -y

# Setup locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen \
 && locale-gen

# Download/extract Plex ALPINE-ARMV7
ENV PLEX_VERSION=1.3.4.3285-b46e0ea PLEX_PATH=/opt/plex/Application
RUN mkdir -p ${PLEX_PATH} /tmp/plex \
 && cd ${PLEX_PATH} \
 && wget --no-verbose -O /tmp/plex/plex.tar https://downloads.plex.tv/plex-media-server/${PLEX_VERSION}/PlexMediaServer-${PLEX_VERSION}-arm7.spk \
 && tar -xvf /tmp/plex/plex.tar -C /tmp/plex \
 && tar -xvf /tmp/plex/package.tgz -C ${PLEX_PATH} \
 && rm -rf /tmp/plex

# Setup volumes
#  - /config holds the server settings
#  - /data is where media should be mounted
#  - /transcode directory for media being transcoded
VOLUME ["/config", "/data", "transcode"]

# Plex config environment vars
ENV HOME=/config

# Plex server port
EXPOSE 32400

# Add a line to the Plex start.sh script to remove any
# previous pid file found in the config dir. Plex wont
# start if this file is left over from a previous run.
RUN sed -i '2i rm -rf /config/Library/Application\\ Support/Plex\\ Media\\ Server/plexmediaserver.pid' ${PLEX_PATH}/Resources/start.sh && mv ${PLEX_PATH}/Resources/start.sh ${PLEX_PATH}/

WORKDIR ${PLEX_PATH}
CMD ["bash", "start.sh"]
