# The base image is the latest 4.x node (LTS) on jessie (debian)
# -onbuild will install the node dependencies found in the project package.json
# and copy its content in /usr/src/app, its WORKDIR
FROM node:6.11.2

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

COPY nodebb /usr/src/app/nodebb
RUN test -d nodebb/src || rm -rf nodebb && git clone -b v1.5.x https://github.com/NodeBB/NodeBB nodebb
RUN npm config set registry http://registry.npmjs.org/
RUN cd nodebb && test -d node_modules || npm install

COPY config.json /tmp
RUN test -f nodebb/config.json || cp /tmp/config.json /usr/src/app/nodebb/
RUN mkdir -p nodebb/local_node_modules

RUN test -d nodebb/local_node_modules/nodebb-plugin-composer-redactor || \
  cd nodebb/local_node_modules && \
  git clone -b master https://github.com/NodeBB-Community/nodebb-plugin-composer-redactor.git

RUN test -d nodebb/local_node_modules/nodebb-plugin-search-elasticsearch || \
  cd nodebb/local_node_modules && \
  git clone -b 1.5.x https://github.com/revir/nodebb-plugin-search-elasticsearch.git

RUN ln -s /usr/src/app/nodebb/local_node_modules/nodebb-plugin-composer-redactor /usr/src/app/nodebb/node_modules/nodebb-plugin-composer-redactor
RUN ln -s /usr/src/app/nodebb/local_node_modules/nodebb-plugin-search-elasticsearch /usr/src/app/nodebb/node_modules/nodebb-plugin-search-elasticsearch
RUN cd /usr/src/app/nodebb/local_node_modules/nodebb-plugin-composer-redactor && npm install
RUN cd /usr/src/app/nodebb/local_node_modules/nodebb-plugin-search-elasticsearch && npm install

ENV NODE_ENV=development

# build nodebb in series, not in parallel, otherwise the nodebb-plugin-composer-redactor
# might get failed, parallel build would make dependencies order wrong.
CMD cd nodebb && ./nodebb setup && ./nodebb dev

# the default port for NodeBB is exposed outside the container
EXPOSE 4567
