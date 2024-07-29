# Stage 1: Build the JAR file
FROM maven:3.8.6-openjdk-17 AS builder

# Set the working directory
WORKDIR /app

# Copy the Maven project files
COPY pom.xml .
COPY src ./src

# Package the application
RUN mvn clean package -DskipTests

# Stage 2: Run the application
FROM eclipse-temurin:17

# Set the working directory
WORKDIR /app

# Copy the JAR file from the builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose the port that the app runs on
EXPOSE 8080

# Set environment variable
ENV SPRING_PROFILES_ACTIVE=docker

# Command to run the JAR file
ENTRYPOINT ["java", "-jar", "app.jar"]
