FROM node:13.2.0-alpine3.10

LABEL author="Yazeed Obaid"
LABEL version=0.1.0
LABEL description="Docker image for Javascript and assests of a PHP Laravel application"

RUN set -x

# working directory is the project root
WORKDIR /opt

# copying the entrypoint script for the container.
COPY docker-entrypoint /usr/bin/docker-entrypoint

# make script executable
RUN chmod +x /usr/bin/docker-entrypoint

ENTRYPOINT ["docker-entrypoint"]
