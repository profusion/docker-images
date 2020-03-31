FROM ravendb/ravendb

# update the repository sources list
# and install dependencies
RUN groupadd --gid 1000 node \
    && useradd --uid 1000 --gid node --shell /bin/bash --create-home node \
    && chown -R node:node /opt/RavenDB/Server \
    && chmod -R 777 /opt/RavenDB/Server \
    && apt-get update \
    && apt-get install -y curl gnupg2 \
    && apt-get -y autoclean

# nvm environment variables
USER node
ENV NVM_DIR /home/node/nvm
ENV NODE_VERSION 12.16.1
ENV YARN_VERSION 1.21.1
ENV RAVEN_DataDir=/home/node/ravenData

RUN mkdir -p $NVM_DIR && curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash

# add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

# install node and npm
RUN /bin/bash -c "source $NVM_DIR/nvm.sh && nvm install $NODE_VERSION && nvm alias default $NODE_VERSION && nvm use default"

RUN curl -o- -L https://yarnpkg.com/install.sh | bash -s -- --version $YARN_VERSION

COPY entrypoint.sh /usr/bin/
ENTRYPOINT ["entrypoint.sh"]

CMD ["node"]
