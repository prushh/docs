# Use the official lightweight Ruby image.
# https://hub.docker.com/_/ruby
FROM ruby:2.6-slim

ARG USER=knative
ARG USER_UID=1001
ARG USER_GID=$USER_UID

ENV BUNDLE_FROZEN=true

# Create and change to the app directory.
WORKDIR "/home/${USER}/app"

# Creates a non-root user to be used exclusively to run the application.
# Install production dependencies.
COPY Gemfile Gemfile.lock ./
RUN groupadd -g $USER_GID $USER && \
    useradd -u $USER_UID -g $USER_GID -m $USER && \
    gem install bundler && bundle install

# Copy local code to the container image.
COPY . ./

# Set the non-root user as current.
USER $USER

# Run the web service on container startup.
CMD ["ruby", "./app.rb"]
