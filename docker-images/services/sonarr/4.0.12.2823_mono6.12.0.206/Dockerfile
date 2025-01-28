# syntax=docker/dockerfile:1

FROM digidanieltech/mono:6.12.0.206-alpine3.21.2

ARG UID="sonarr"
ARG GID="media"

ENV INSTALL_DIR="/opt"
ENV APP_DIR="${INSTALL_DIR}/Sonarr"
ENV DATA_DIR="/var/lib/sonarr"

# Setup user and group
RUN addgroup "${GID}"
RUN adduser --system --no-create-home -G "${GID}" "${UID}"

RUN apk update && apk add --no-cache tar curl sqlite openrc
RUN mkdir -p /run/openrc && \
    touch /run/openrc/softlevel

RUN mkdir -p "${DATA_DIR}"
RUN chown -R "${UID}":"${GID}" "${DATA_DIR}"
RUN chmod 775 "${DATA_DIR}"

RUN mkdir /build
ADD https://services.sonarr.tv/v1/download/main/latest?version=4&os=linux&arch=x64 /build/sonarr.tar.gz
RUN tar -xvzf /build/sonarr.tar.gz -C "${INSTALL_DIR}"
RUN rm -rf /build

RUN chown "${UID}":"${GID}" -R "${APP_DIR}"
RUN chmod 775 "${APP_DIR}"

RUN cat <<EOF | tee /etc/init.d/sonarr >/dev/null
#!/sbin/openrc-run
name="Sonarr Daemon"
description="Sonarr Daemon"
command="${APP_DIR}/Sonarr -nobrowser -data=${DATA_DIR}"
command_background=true
pidfile="/run/sonarr.pid"
EOF

# RUN cat <<EOF | tee /etc/systemd/system/Sonarr.service >/dev/null
# [Unit]
# Description=Sonarr Daemon
# After=syslog.target network.target
# [Service]
# User=${UID}
# Group=${GID}
# UMask=0002
# Type=simple
# ExecStart=${APP_DIR}/Sonarr -nobrowser -data=${DATA_DIR}
# TimeoutStopSec=20
# KillMode=process
# Restart=on-failure
# [Install]
# WantedBy=multi-user.target
# EOF

RUN chmod +x /etc/init.d/sonarr

RUN rc-update add sonarr default

WORKDIR "${APP_DIR}"
