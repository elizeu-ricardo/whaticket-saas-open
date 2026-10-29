FROM node:20

WORKDIR /usr/src/app
COPY . .

ENV DOCKERIZE_VERSION v0.7.0
RUN wget -O /usr/local/bin/dumb-init https://github.com/Yelp/dumb-init/releases/download/v1.2.5/dumb-init_1.2.5_x86_64

RUN chmod +x /usr/local/bin/dumb-init

RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

RUN tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

RUN rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

RUN npm config set maxsockets=5

RUN npm install

RUN npm run build

ENV NODE_ENV=production
ENV PORT=3000

EXPOSE 3000

ENTRYPOINT ["dumb-init", "--"]
CMD dockerize -wait tcp://${DB_HOST}:5432 -timeout 60s \
  && npx sequelize db:migrate \
  && npx sequelize db:seed:all \
  && node dist/server.js
