FROM            maven:alpine AS build
WORKDIR         /build
COPY            pom.xml ./
RUN             mvn -B dependency:go-offline
COPY            src ./src
RUN             mvn -B -DskipTests package spring-boot:repackage

FROM            openjdk:8-jre-alpine
WORKDIR         /app
EXPOSE          8080
COPY --from=build /build/target/app.jar /app/app.jar
CMD             ["java", "-jar", "/app/app.jar"]
HEALTHCHECK --interval=10s --retries=5 CMD ["wget", "-qO-", "http://localhost:8080/actuator/health"]