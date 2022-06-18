FROM node:16-alpine3.12 as build-stage

WORKDIR /usr/src/app

COPY package.json .
COPY yarn.lock .

RUN yarn cache clean
RUN yarn install

COPY . .

RUN yarn prod:build

CMD ["yarn","run","dev:next"]
ENTRYPOINT ["yarn","run","dev:next"]

FROM nginx:1.20

# ビルドした成果物を Docker 上の Nginx のドキュメントとして扱うためにコピー(デプロイ)
COPY --from=build-stage /usr/src/app/out /usr/share/nginx/html

# Nginx の設定ファイルを Docker 上の Nginx にコピー
COPY ./docker/nginx/nginx.conf /etc/nginx/nginx.conf

EXPOSE 4000
