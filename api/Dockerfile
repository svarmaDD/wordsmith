# Build stage
FROM --platform=${BUILDPLATFORM} maven:3-amazoncorretto-20 as build
WORKDIR /usr/local/app
COPY pom.xml .
RUN mvn verify -DskipTests --fail-never
COPY src ./src
RUN mvn verify

# Run stage
FROM --platform=${TARGETPLATFORM} amazoncorretto:20
WORKDIR /usr/local/app
COPY --from=build /usr/local/app/target/*.jar app.jar 
ADD "https://dtdg.co/latest-java-tracer" dd-java-agent.jar
ENTRYPOINT ["java","-javaagent:dd-java-agent.jar", "-Xmx8m", "-Xms8m", "-jar", "app.jar"]
EXPOSE 8080
