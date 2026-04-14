FROM eclipse-temurin:21-jdk AS builder
WORKDIR /build

COPY pom.xml ./pom.xml
COPY shop-common ./shop-common
COPY payment-service/pom.xml ./payment-service/pom.xml

RUN apt-get update && apt-get install -y maven && \
    mvn -pl shop-common -am install -DskipTests -q && \
    mvn -pl payment-service dependency:go-offline -q

COPY payment-service/src ./payment-service/src
RUN mvn -pl payment-service package -DskipTests -q

FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=builder /build/payment-service/target/*.jar app.jar
RUN mkdir -p /opt/apm
EXPOSE 8083
ENTRYPOINT ["java", "-jar", "app.jar"]
