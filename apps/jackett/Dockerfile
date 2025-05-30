ARG VERSION
FROM docker.io/library/alpine:3.22

ARG TARGETPLATFORM
ARG VERSION
ARG CHANNEL

ENV XDG_CONFIG_HOME=/config
ENV UMASK="0002" \
    TZ="Etc/UTC"

USER root
WORKDIR /app

#hadolint ignore=DL3018,DL3059
RUN \
    apk add --no-cache \
        bash \
        ca-certificates \
        catatonit \
        curl \
        icu-libs \
        jq \
        libintl \
        nano \
        sqlite-libs \
        tzdata \
    && \
    case "${TARGETPLATFORM}" in \
        'linux/amd64') export ARCH='AMDx64' ;; \
        'linux/arm64') export ARCH='ARM64' ;; \
    esac \
    && \
    curl -fsSL "https://github.com/Jackett/Jackett/releases/download/v${VERSION}/Jackett.Binaries.LinuxMusl${ARCH}.tar.gz" \
        | tar xzf - -C /app --strip-components 1 \
    && rm -rf /app/JackettUpdater* \
    && chown -R root:root /app \
    && chmod -R 755 /app \
    && rm -rf /tmp/*

COPY ./apps/jackett/entrypoint.sh /entrypoint.sh

USER nobody:nogroup
WORKDIR /config
VOLUME ["/config"]

ENTRYPOINT [ "/usr/bin/catatonit", "--" ]
CMD ["/entrypoint.sh"]

LABEL org.opencontainers.image.source="https://github.com/Jackett/Jackett"
