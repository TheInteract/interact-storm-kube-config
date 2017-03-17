FROM openjdk:8-jre-alpine

# Install required packages
RUN apk add --no-cache \
    bash \
    python \
    su-exec

ENV STORM_USER=storm \
    STORM_CONF_DIR=/conf \
    STORM_DATA_DIR=/data \
    STORM_LOG_DIR=/logs

# Add a user and make dirs
RUN set -x \
    && mkdir -p "$STORM_CONF_DIR" "$STORM_DATA_DIR" "$STORM_LOG_DIR"

COPY apache-storm-1.0.3.tar.gz /

ARG GPG_KEY=ACEFE18DD2322E1E84587A148DE03962E80B8FFD
ARG DISTRO_NAME=apache-storm-1.0.3

# Download Apache Storm, verify its PGP signature, untar and clean up
RUN set -x \
    && apk add --no-cache --virtual .build-deps \
        gnupg \
    && wget -q "http://www.apache.org/dist/storm/$DISTRO_NAME/$DISTRO_NAME.tar.gz.asc" \
    && export GNUPGHOME="$(mktemp -d)" \
    && gpg --keyserver ha.pool.sks-keyservers.net --recv-key "$GPG_KEY" \
    && gpg --batch --verify "$DISTRO_NAME.tar.gz.asc" "$DISTRO_NAME.tar.gz" \
    && tar -xzf "$DISTRO_NAME.tar.gz" \
    && rm -r "$GNUPGHOME" "$DISTRO_NAME.tar.gz" "$DISTRO_NAME.tar.gz.asc" \
    && apk del .build-deps

WORKDIR $DISTRO_NAME

ENV PATH $PATH:/$DISTRO_NAME/bin

ADD configure.sh /

RUN chmod +x /configure.sh