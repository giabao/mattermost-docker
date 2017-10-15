FROM postgres:9.6-alpine

RUN apk update \
    && apk add --no-cache \
      ca-certificates gcc \
      linux-headers \
      musl-dev \
      libffi \
      libffi-dev \
      openssl \
      openssl-dev \
      bash \
      python \
      python-dev \
    && apk add py-pip \
    && pip install --upgrade pip \
    && pip install 'wal-e<1.0.0' \
    && apk del \
      gcc \
      linux-headers \
      musl-dev \
      libffi-dev \
      openssl-dev \
      python-dev

# Add wale script
COPY setup-wale.sh /docker-entrypoint-initdb.d/

# Add and configure entrypoint and command
COPY entrypoint.sh /
ENTRYPOINT ["/entrypoint.sh"]
CMD ["postgres"]
