#FROM java:8
FROM  docker.m.daocloud.io/library/eclipse-temurin:8-jdk
ARG JAR_FILE
COPY ${JAR_FILE} app.jar
EXPOSE 10086
ENTRYPOINT ["java","-jar","/app.jar"]
