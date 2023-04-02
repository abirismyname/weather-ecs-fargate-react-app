FROM node:17

WORKDIR /home/node/app

COPY package.json yarn.lock ./
RUN yarn install

COPY tsconfig.json  ./
COPY tsconfig.node.json  ./
COPY *.cjs  ./
COPY index.html  vite.config.ts ./
COPY src ./src

RUN yarn build
