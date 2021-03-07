FROM node:14-alpine as builder
RUN apk add git
ARG frontend_branch
ENV frontend_branch=${frontend_branch}
ENV VUE_APP_IS_DOCKER=true
WORKDIR /usr/src/penguin-frontend

# get source code
# COPY . .
# RUN git clone https://github.com/penguin-statistics/frontend-v2.git -b ${frontend_branch} .
RUN git clone https://github.com.cnpmjs.org/penguin-statistics/frontend-v2.git -b ${frontend_branch} .
#RUN git clone https://github.com.cnpmjs.org/bbaa-bbaa/frontend-v2.git -b ${frontend_branch} .

# yarn build dist folder
RUN yarn install
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
