# Build stage: Use Maven official image (with JDK 17)
FROM maven:3.9.3-eclipse-temurin-17 AS build

WORKDIR /app

# Copy pom.xml and source code
COPY ./pom.xml /app/
COPY ./src /app/src/

# Build the application skipping tests
RUN mvn clean package -Dmaven.test.skip=true

# Runtime stage: Use Amazon Corretto 17 JDK image (no Maven here)
FROM public.ecr.aws/amazoncorretto/amazoncorretto:17

WORKDIR /app

# Copy the built jar from the build stage
COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080

CMD ["java", "-jar", "app.jar"]
