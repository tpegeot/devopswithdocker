# Use ubuntu 16.04 as base image
FROM ubuntu:16.04 

# Set workdir
WORKDIR /mydir 

# Copy result of git clone into container
COPY frontend-example-docker/ . 

# Install curl as it is required to install nodejs
RUN apt-get update && apt-get install -y curl

# Setup nodejs repo
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash 

# Install nodejs
RUN apt install -y nodejs

# Check node and npm version
RUN node -v
RUN npm -v

# Install all npm modules
RUN npm install

# Expose port 5000
EXPOSE 5000

# Start node server
CMD ["/usr/bin/npm","start"]
