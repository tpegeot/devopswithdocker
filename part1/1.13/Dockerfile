# From openjdk as we need Java
FROM openjdk:8

# Set workdir
WORKDIR /mydir

# Copy result of git clone into container
COPY spring-example-project/ .

# Build the project 
RUN /bin/sh mvnw package

# Expose application on port 8000
EXPOSE 8000

# Run application
CMD ["java","-jar","./target/docker-example-1.1.3.jar"]

