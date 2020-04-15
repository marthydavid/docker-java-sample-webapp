FROM maven:3-jdk-8 as builder
COPY . /tmp
WORKDIR /tmp
RUN mvn clean install

FROM bitnami/tomcat:latest

COPY --from=builder /tmp/target/docker-java-sample-webapp-1.0-SNAPSHOT.war /bitnami/tomcat/data/
