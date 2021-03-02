FROM node:14-slim as builder
ENV VUE_APP_IS_DOCKER=true
WORKDIR /usr/src/penguin-frontend

# yarn build dist folder
ADD source/package.json /usr/src/penguin-frontend/
ADD source/yarn.lock /usr/src/penguin-frontend/
RUN yarn install

COPY source /usr/src/penguin-frontend/
RUN yarn build

# nginx setting
FROM nginx:alpine
RUN set -x ; \
    addgroup -g 82 -S www-data ; \
    adduser -u 82 -D -S -G www-data www-data && exit 0 ; exit 1
COPY --from=builder /usr/src/penguin-frontend/dist/ /var/www/html/
COPY nginx.conf /etc/nginx/

# start nginx server
EXPOSE 80

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]
