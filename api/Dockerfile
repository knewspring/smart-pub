#FROM ubuntu:20.04
FROM node:latest
FROM mysql
FROM mongo

LABEL maintainer="Jason Sprint"

ARG WWWGROUP
USER root

WORKDIR /var/www/html/smartpub/api

EXPOSE 3000 3001 3002 3003 3004

ENV DEBIAN_FRONTEND noninteractive
ENV TZ=UTC

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#install mysql
RUN apt install mysql-server && systemctl start mysqld

COPY . .

RUN export NODE_ENV=development \
    && npm config set registry https://registry.npm.taobao.org/ \
    && cd ./Common/Sequelize-cli && npx sequelize-cli db:create && npx sequelize-cli db:migrate && npx sequelize-cli db:seed --seed 20210303111816-initialize_application_informations.cjs \
    && npm install -g pm2 \
    && cd ./User && npm install \
    && cd ../Feeds && npm install \
    && cd ../Common && npm install \
    && cd ../Update && npm install \
    && cd ../Publish && npm install \
    && cd ../Notification && npm install \
    && cd .. \
    && cd ./User && npm run swagger \
    && cd ../Feeds && npm run swagger  \
    && cd ../Publish && npm run swagger  \
    && cd ../Notification && npm run swagger  \
    && cd ../Update && npm run swagger \
    && cd .. \

#COPY ./start-api /usr/local/bin/start-api
#RUN chmod +x /usr/local/bin/start-container

#ENTRYPOINT ["start-api"]

CMD sh start-apiqu

