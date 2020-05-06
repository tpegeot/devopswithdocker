# Use node image as a base image
FROM node

# Set workdir
WORKDIR /mydir 

# Copy result of git clone into container
COPY deplacement-covid-19 .

# Check node and npm version
RUN /usr/local/bin/node -v
RUN /usr/local/bin/npm -v

# Install all npm modules
RUN /usr/local/bin/npm install

# Expose port 1234
EXPOSE 1234

# Start node server
CMD ["/usr/local/bin/npm","start"]
