# syntax = docker/dockerfile:experimental
FROM maven:3.6.3-jdk-8-openj9 AS builder
ARG backend_branch
ENV backend_branch=${backend_branch}
RUN apt update && apt -y install git
WORKDIR /usr/src/penguin-backend

# get source code
# COPY . .
# RUN git clone https://github.com/penguin-statistics/backend.git -b ${backend_branch} .
RUN git clone https://github.com.cnpmjs.org/penguin-statistics/backend.git -b ${backend_branch} .

#COPY settings.xml /usr/share/maven/conf/settings.xml
COPY application.yml /usr/src/penguin-backend/src/main/resources/application.yml
RUN --mount=type=cache,target=/root/.m2,id=penguin_backend,sharing=locked \
    cd /usr/src/penguin-backend \ 
    #&& export MAVEN_OPTS="-DsocksProxyHost=172.18.0.1 -DsocksProxyPort=1081" \
    && mvn dependency:go-offline
RUN --mount=type=cache,target=/root/.m2,id=penguin_backend,sharing=locked \
    cd /usr/src/penguin-backend \
    # && export MAVEN_OPTS="-DsocksProxyHost=172.18.0.1 -DsocksProxyPort=1081" \
    && mvn clean package -Dmaven.test.skip=true

FROM tomcat:9.0.43-jdk8-adoptopenjdk-openj9
COPY  --from=builder /usr/src/penguin-backend/target/PenguinStats.war /usr/local/tomcat/webapps/PenguinStats.war
EXPOSE 8080
CMD ["catalina.sh", "run"]
