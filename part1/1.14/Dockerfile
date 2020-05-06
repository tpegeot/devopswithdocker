# From ruby base image as we'll run a ruby image
FROM ruby:2.6.0

# Set workdir
WORKDIR /mydir

# Copy result of git clone into container
COPY rails-example-project .

# Install curl as it is required to install nodejs
RUN apt-get update && apt-get install -y curl

# Setup nodejs repo
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash

# Install nodejs
RUN apt install -y nodejs

# Install bundler
RUN gem install bundler

# install all dependencies specified in the Gemfile
RUN bundle install 

# Set RAILS_ENV variable
ENV RAILS_ENV=production

# Set SECRET_KEY_BASE variable
ENV SECRET_KEY_BASE 0d5vXr5c7R8YfyyE+Dw9+z8QiqS6npdHk2I96ge0Cq0JYdHYxNqddIzChcIraLs//Dsd3daYGGwX3yie5bSl0IWivCWQ2J7dSrQKw6DJYbrTMPfeCbwJT6LXug5lvd2wyQkKz9CEldjZEuoey/thQug7qkXvHoX8Udl9TKFjapvFpNz2vYUJ6b6xBV/v2jayvMoOuf17gRdItXR2c/q7wRaWLjuCu9OAReMDWNxNeY/UFOhZPIZQKjUMEUpzX5FYFV/IuM2DdqPy/vv9V4wF4lLfXi7OTBzOiC+9LWrRIZ9GxUnC6VvXqK0KcDnQBkCurmizSBVRbeebxlMuZx9db1Q5Yw8Cagj+wOoXgp4rQsTItcOJQ615A8tVpuoXyvoGAc+JMHzzbKkDsik02gh1PoD+nEOBFWxtHXpt--zqE6wZaIOMNAUukB--8fNV5cbeq2eZVMKCRBQEqg==

# Run migrations
RUN bin/rails db:migrate

# Precompile assets
RUN bin/rake assets:precompile

# Expose application on port 8000
EXPOSE 3000

# Run application
CMD ["bin/rails","s","-e","production"]


