FROM node:14.3.0-alpine3.11

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY ./node-files/package*.json ./

RUN npm install

# Bundle app source
COPY ./node-files/ ./

EXPOSE 3000
CMD [ "node", "index.js" ]
