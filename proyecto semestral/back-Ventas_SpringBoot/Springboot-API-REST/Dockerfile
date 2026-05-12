
FROM maven:3.9-eclipse-temurin-17-alpine AS builder

WORKDIR /app

COPY pom.xml ./

RUN mvn dependency:go-offline -B

COPY src ./src

RUN mvn clean package -DskipTests -B

FROM eclipse-temurin:17-jre-alpine

RUN addgroup -g 1001 -S appgroup && \
    adduser -S -D -H -u 1001 -h /home/appuser -s /sbin/nologin appuser

WORKDIR /app

COPY --from=builder /app/target/*.jar app.jar

RUN chown -R appuser:appgroup /app && \
    chmod -R 755 /app

USER appuser

EXPOSE 8080

CMD ["java", "-Djava.security.egd=file:/dev/./urandom", "-jar", "app.jar"]
