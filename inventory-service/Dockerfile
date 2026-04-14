FROM eclipse-temurin:21-jdk AS builder
WORKDIR /build

COPY pom.xml ./pom.xml
COPY shop-common ./shop-common
COPY inventory-service/pom.xml ./inventory-service/pom.xml

RUN apt-get update && apt-get install -y maven && \
    mvn -pl shop-common -am install -DskipTests -q && \
    mvn -pl inventory-service dependency:go-offline -q

COPY inventory-service/src ./inventory-service/src
RUN mvn -pl inventory-service package -DskipTests -q

FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=builder /build/inventory-service/target/*.jar app.jar
RUN mkdir -p /opt/apm
EXPOSE 8082
ENTRYPOINT ["java", "-jar", "app.jar"]
