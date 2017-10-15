FROM alpine:3.5

# Some ENV variables
ENV PATH="/mattermost/bin:${PATH}"
ENV MM_VERSION=4.3.0

# Build argument to set Mattermost edition
#ARG edition=entreprise
ARG edition=team

# Install some needed packages
RUN apk add --update \
      curl \
      jq \
      netcat-openbsd \
      ca-certificates \
    && rm -rf /var/cache/apk/*

# Get Mattermost
RUN mkdir -p /mattermost/data \
    && if [ "$edition" = "team" ] ; then curl https://releases.mattermost.com/$MM_VERSION/mattermost-team-$MM_VERSION-linux-amd64.tar.gz | tar -xvz ; \
      else curl https://releases.mattermost.com/$MM_VERSION/mattermost-$MM_VERSION-linux-amd64.tar.gz | tar -xvz ; fi \
    && mkdir /lib64 && ln -s /lib/libc.musl-x86_64.so.1 /lib64/ld-linux-x86-64.so.2 \
    && cp /mattermost/config/config.json /config.json.save \
    && rm -rf /mattermost/config/config.json

# Configure entrypoint and command
COPY entrypoint.sh /
ENTRYPOINT ["/entrypoint.sh"]
WORKDIR /mattermost/bin
CMD ["platform"]

# Expose port 80 of the container
EXPOSE 80

# Use a volume for the data directory
VOLUME /mattermost/data
