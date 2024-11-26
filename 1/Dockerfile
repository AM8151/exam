FROM maven:3.6.3-openjdk-8 AS builder
WORKDIR /app
COPY pom.xml .
COPY src ./src
COPY LICENSE-GPLv3.txt ./LICENSE-GPLv3.txt
RUN mvn clean package -DskipTests
RUN mvn stampo:build

FROM openjdk:8-jre-alpine AS runtime
WORKDIR /app
COPY --from=builder /app/target/lavagna-jetty-console.war /app/lavagna-jetty-console.war
COPY --from=builder /app/target/lavagna/help /app/target/lavagna/help
COPY entrypoint.sh ./entrypoint.sh
RUN chmod +x ./entrypoint.sh && \
    apk add --no-cache netcat-openbsd
RUN adduser -D appuser && \
    chown -R appuser:appuser /app
USER appuser
ENTRYPOINT ["./entrypoint.sh"]