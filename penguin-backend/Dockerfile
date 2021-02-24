FROM maven:3.6.3-jdk-8-openj9 AS builder
RUN mkdir /usr/src/penguin-backend \
    && cd /usr/src/penguin-backend \
  # && export http_proxy=http://172.17.0.1:1087 \
  # && export https_proxy=http://172.17.0.1:1087 \
    && apt update \
    && apt -y install git \
    && git clone https://github.com/penguin-statistics/backend.git .
COPY application.yml /usr/src/penguin-backend/src/main/resources/application.yml
RUN cd /usr/src/penguin-backend \
  # && export MAVEN_OPTS="-DsocksProxyHost=172.17.0.1 -DsocksProxyPort=1081" \
    && mvn clean package -Dmaven.test.skip=true
FROM tomcat:9.0.43-jdk8-adoptopenjdk-openj9
COPY  --from=builder /usr/src/penguin-backend/target/PenguinStats.war /usr/local/tomcat/webapps/PenguinStats.war