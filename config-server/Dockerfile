FROM eclipse-temurin:17-jre-jammy
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

WORKDIR /app

ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar

# Copy config files into the container
COPY configs/ /app/configs/

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
    CMD curl --fail http://localhost:8888/actuator/health || exit 1

EXPOSE 8888

ENTRYPOINT ["java", "-jar", "app.jar"]
