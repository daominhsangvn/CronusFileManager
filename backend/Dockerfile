FROM node:latest

EXPOSE 3131

RUN mkdir -p /usr/src/app

WORKDIR /usr/src/app
COPY package.json package.json

RUN npm install

COPY . .

CMD ["node", "./server.js"]
