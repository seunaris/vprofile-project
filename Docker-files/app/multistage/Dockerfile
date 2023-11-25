# Use a smaller base image for the build stage
FROM maven:3.8.4-openjdk-8 AS BUILD_IMAGE

WORKDIR /app

# Only copy the pom.xml to leverage Docker cache
COPY pom.xml .

# Download dependencies, if the pom.xml changes
RUN mvn dependency:go-offline

# Copy the rest of the application code
COPY src ./src

# Build the application
RUN mvn package

# Use a smaller base image for the runtime stage
FROM tomcat:9-jre11

# Remove default Tomcat webapps
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the built WAR file from the build stage
COPY --from=BUILD_IMAGE /app/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

# Expose port 8080
EXPOSE 8080

# Start Tomcat
CMD ["catalina.sh", "run"]