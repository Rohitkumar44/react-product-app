# Use the official Amazon Corretto 17 image as the base image for build stage
FROM public.ecr.aws/amazoncorretto/amazoncorretto:17 AS build

# Set the working directory
WORKDIR /app

# Copy the pom.xml and source code
COPY ./pom.xml /app
COPY ./src /app/src

# Build the application using Maven wrapper if you have it, otherwise Maven should be installed in the build image
RUN ./mvnw clean package -Dmaven.test.skip=true || mvn clean package -Dmaven.test.skip=true

# Use Amazon Corretto 17 non-alpine image as the runtime base image
FROM public.ecr.aws/amazoncorretto/amazoncorretto:17

# Set the working directory in the runtime image
WORKDIR /app

# Copy the built jar file from the build stage
COPY --from=build /app/target/*.jar app.jar

# Expose the port on which the Spring Boot application will run
EXPOSE 8080

# Command to run the application
CMD ["java", "-jar", "app.jar"]
