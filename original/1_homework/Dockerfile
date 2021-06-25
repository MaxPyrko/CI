# FROM openjdk:16-alpine3.13
# WORKDIR /app
# COPY pom.xml ./
# RUN ./mvnw dependency:go-offline
# COPY src ./src
# CMD ["./App.java", "spring-boot:run"]



# FROM openjdk:8-jdk-alpine
# # ----
# # Install Maven
# RUN apk add --no-cache curl tar bash
# ARG MAVEN_VERSION=3.3.9
# ARG USER_HOME_DIR="/root"
# RUN mkdir -p /usr/share/maven && \
# curl -fsSL http://apache.osuosl.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar -xzC /usr/share/maven --strip-components=1 && \
# ln -s /usr/share/maven/bin/mvn /usr/bin/mvn
# ENV MAVEN_HOME /usr/share/maven
# ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"
# # speed up Maven JVM a bit
# ENV MAVEN_OPTS="-XX:+TieredCompilation -XX:TieredStopAtLevel=1"
# ENTRYPOINT ["/usr/bin/mvn"]
# # ----
# # Install project dependencies and keep sources
# # make source folder
# RUN mkdir -p /usr/src/app
# WORKDIR /usr/src/app
# # install maven dependency packages (keep in image)
# COPY pom.xml /usr/src/app
# RUN mvn -T 1C install && rm -rf target
# # copy other source files (keep in image)
# COPY src /usr/src/app/src



# FROM maven:3.5.2-jdk-8-alpine AS MAVEN_TOOL_CHAIN
# COPY pom.xml /tmp/
# RUN mvn -B dependency:go-offline -f /tmp/pom.xml -s /usr/share/maven/ref/settings-docker.xml
# COPY src /tmp/src/
# WORKDIR /tmp/
# RUN mvn -B -s /usr/share/maven/ref/settings-docker.xml package

# FROM java:8-jre-alpine

# EXPOSE 8080

# RUN mkdir /app
# COPY --from=MAVEN_TOOL_CHAIN /tmp/target/*.jar /app/spring-boot-application.jar

# ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app/spring-boot-application.jar"]




FROM maven:3.6.0-jdk-11-slim AS build
COPY src /home/app/src
COPY pom.xml /home/app
RUN mvn -f /home/app/pom.xml clean package
#
# Package stage
#
FROM openjdk:11-jre-slim
COPY --from=build /home/app/target/my-app-1.0-SNAPSHOT.jar /usr/local/lib/demo.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/usr/local/lib/demo.jar"]