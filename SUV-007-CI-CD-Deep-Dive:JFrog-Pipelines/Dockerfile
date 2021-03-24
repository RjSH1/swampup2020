#Download image from artifactory. <-- Change IP to that of your Artifactory Server
FROM 35.243.193.12:8082/docker/openjdk:11-jdk

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y nodejs \
    npm                       # note this one

WORKDIR /app

MAINTAINER Colathur Vijayan

# Download artifacts from Artifactory <-- Change IP to that of your Artifactory Server
RUN curl http://35.243.193.12:8082/artifactory/libs-release-local/com/jfrog/backend/1.0.0/backend-1.0.0.jar --output server1.jar
RUN curl http://35.243.193.12:8082/artifactory/npm-dev-local/frontend/-/frontend-3.0.0.tgz --output client1.tgz

#Extract vue app
RUN tar -xzf client1.tgz && rm client1.tgz

WORKDIR "package"

RUN npm install

RUN npm run build

# Set JAVA OPTS + Static file location
ENV STATIC_FILE_LOCATION="/app/package/target/dist/"
ENV JAVA_OPTS=""

# Fire up our Spring Boot app
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -Dspring.profiles.active=remote -Djava.security.egd=file:/dev/./urandom -jar /app/server1.jar" ]
