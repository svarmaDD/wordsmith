# Build stage
FROM --platform=${BUILDPLATFORM} maven:3-amazoncorretto-20 as build
WORKDIR /usr/local/app
COPY pom.xml .
RUN mvn dependency:go-offline
# RUN mvn verify -DskipTests --fail-never
COPY src ./src
RUN mvn package -DskipTests
# RUN mvn verify

# Run stage
FROM --platform=${TARGETPLATFORM} amazoncorretto:20
WORKDIR /usr/local/app
COPY --from=build /usr/local/app/target .
RUN curl -Lo dd-java-agent.jar 'https://dtdg.co/latest-java-tracer'
ENTRYPOINT ["java", "-javaagent:../dd-java-agent.jar", "-Ddd.trace.sample.rate=1", "-Xmx16m", "-Xms16m", "-jar", "/usr/local/app/words.jar"]
# COPY --from=build /usr/local/app/target/*.jar app.jar 
# ADD "https://dtdg.co/latest-java-tracer" dd-java-agent.jar
# ENTRYPOINT ["java","-javaagent:dd-java-agent.jar", "-jar", "app.jar"]
EXPOSE 8080
