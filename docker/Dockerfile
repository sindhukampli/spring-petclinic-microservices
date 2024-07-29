# Stage 1: Build the JAR file
FROM maven:3.8.6-openjdk-11 AS builder

# Set the working directory
WORKDIR /app

# Copy the pom.xml and src directory to the container
COPY pom.xml .
COPY src ./src

# Build the application
RUN mvn clean package -DskipTests

# Stage 2: Run the application
FROM eclipse-temurin:17

# Set the working directory
WORKDIR /app

# Copy the built JAR file from the builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose the port that the application will run on
EXPOSE 8080

# Set environment variables
ENV SPRING_PROFILES_ACTIVE=docker

# Define the entry point for the container
ENTRYPOINT ["java", "-jar", "app.jar"]
