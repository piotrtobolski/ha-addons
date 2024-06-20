ARG BUILD_FROM
FROM $BUILD_FROM

ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

ARG BASHIO_VERSION
ARG TEMPIO_VERSION
ARG BUILD_ARCH

RUN set -x \
    && apt-get update && apt-get install -y --no-install-recommends \
    bash \
    jq \
    tzdata \
    curl \
    ca-certificates \
    xz-utils \
    && curl -L -f -s -o /usr/bin/tempio \
    "https://github.com/home-assistant/tempio/releases/download/${TEMPIO_VERSION}/tempio_${BUILD_ARCH}" \
    && chmod a+x /usr/bin/tempio \
    \
    && mkdir -p /usr/src/bashio \
    && curl -L -f -s "https://github.com/hassio-addons/bashio/archive/v${BASHIO_VERSION}.tar.gz" \
    | tar -xzf - --strip 1 -C /usr/src/bashio \
    && mv /usr/src/bashio/lib /usr/lib/bashio \
    && ln -s /usr/lib/bashio/bashio /usr/bin/bashio \
    && rm -rf /usr/src/*

# ensure entrypoint from base isn't run
ENTRYPOINT [ "bashio" ]

# Copy data for add-on
COPY run.sh /
RUN chmod a+x /run.sh


CMD [ "/run.sh" ]

# Health check
# HEALTHCHECK --interval=5m --timeout=3s --start-period=90s \
#     CMD nc -z localhost 8888 || exit 1