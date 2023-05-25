FROM debian:buster

ENV version=5.34.2

# Install the PHP extensions we need
RUN set -eux; \
	apt-get update; \
	apt-get install -y --no-install-recommends \
	ca-certificates \
	openssl \
	curl \
	gnupg

RUN apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
	rm -rf /var/lib/apt/lists/*

# Get release
ADD https://releases.mattermost.com/${version}/mattermost-${version}-linux-amd64.tar.gz /

RUN tar -xzf /mattermost-${version}-linux-amd64.tar.gz
RUN mv /mattermost /opt/.

RUN rm -rf /mattermost-${version}-linux-amd64.tar.gz

# Add correct configuration
COPY config.json /opt/mattermost/config/config.json

# Set up a system user and group called mattermost that will run this service, and set the ownership and permissions.
RUN useradd --system --user-group mattermost
RUN chown -R mattermost:mattermost /opt/mattermost
RUN chmod -R g+w /opt/mattermost

USER mattermost:mattermost

VOLUME /opt/mattermost/data

CMD ["/opt/mattermost/bin/mattermost"]