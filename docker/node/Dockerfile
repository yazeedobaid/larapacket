FROM node:15.0.1-alpine3.12

LABEL author="Yazeed Obaid"
LABEL version=1.0.0
LABEL description="Docker image for Javascript and assests of a PHP Laravel application"

RUN set -x
RUN apk update && apk upgrade && \
    apk add --no-cache bash git openssh

RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app

# working directory is the project root
WORKDIR /home/node/app

# copying the entrypoint script for the container.
COPY docker-entrypoint /usr/bin/docker-entrypoint

# make script executable
RUN chmod +x /usr/bin/docker-entrypoint

# set user to node user
USER node

ENTRYPOINT ["docker-entrypoint"]
