#### Use nodered/node-red:2.2.2 as the base
FROM nodered/node-red:2.2.2 AS base

USER root

# Copy package.json contains curated version of Node-RED NPM module and node dependencies
COPY package.json .

# Install node-red nodes
RUN npm install --unsafe-perm --no-update-notifier --no-fund --only=production

USER node-red
