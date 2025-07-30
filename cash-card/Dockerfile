# Stage 1: Build the application using a JDK image
FROM eclipse-temurin:17-jdk-jammy as builder
WORKDIR /workspace
# Copy the gradle wrapper and build files first to leverage Docker layer caching
COPY gradlew .
COPY gradle gradle
COPY build.gradle .
# settings.gradle might not exist in all projects, copy if it does
COPY settings.gradle .
# Copy the source code
COPY src src
# Build the application, disable the daemon for CI environments
RUN ./gradlew build --no-daemon

# Stage 2: Create the final, smaller image using a JRE image
FROM eclipse-temurin:17-jre-jammy
WORKDIR /app
# Copy only the built JAR file from the builder stage
COPY --from=builder /workspace/build/libs/*.jar app.jar
# Set the entrypoint to run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
