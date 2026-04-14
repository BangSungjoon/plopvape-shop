FROM eclipse-temurin:21-jdk AS builder
WORKDIR /build

COPY pom.xml ./pom.xml
COPY shop-common ./shop-common
COPY notification-service/pom.xml ./notification-service/pom.xml

RUN apt-get update && apt-get install -y maven && \
    mvn -pl shop-common -am install -DskipTests -q && \
    mvn -pl notification-service dependency:go-offline -q

COPY notification-service/src ./notification-service/src
RUN mvn -pl notification-service package -DskipTests -q

FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=builder /build/notification-service/target/*.jar app.jar
RUN mkdir -p /opt/apm
EXPOSE 8084
ENTRYPOINT ["java", "-jar", "app.jar"]
