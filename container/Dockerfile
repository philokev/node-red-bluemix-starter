FROM node:6-alpine

# Install the packages to build blockchain composer dependencies
RUN apk add --no-cache python make g++ libc6-compat git

# User data directory, contains flows, config and nodes.
RUN mkdir /data
RUN mkdir /usr/src

# Add node-red user so we aren't running as root.
RUN chown -R node:node /data \
    && ln -s /home/node /usr/src/node-red \
	&& chown -R node:node /usr/src/node-red 

# Get custom Watson IoT module for Containers
RUN git clone https://github.com/philokev/node-red-contrib-scx-ibmiotapp /usr/src/node-red-contrib-scx-ibmiotapp
WORKDIR /usr/src/node-red-contrib-scx-ibmiotapp
RUN npm link

# package.json contains Node-RED NPM module and node dependencies
RUN git clone https://github.com/philokev/node-red-bluemix-starter /usr/src/node-red

USER node
WORKDIR /usr/src/node-red

RUN npm link
RUN npm install

# User configuration directory volume
EXPOSE 1880

# Environment variable holding file path for flows configuration
ENV FLOWS=flows.json
ENV NODE_PATH=/usr/src/node-red/node_modules:/data/node_modules

CMD ["npm", "start", "--", "--userDir", "/data"]
