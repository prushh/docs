# Use the official lightweight Node.js 12 image.
# https://hub.docker.com/_/node
FROM node:12-slim

ARG USER=knative
ARG USER_UID=1001
ARG USER_GID=$USER_UID

# Create and change to the app directory.
WORKDIR "/home/${USER}/app"

# Copy application dependency manifests to the container image.
# A wildcard is used to ensure both package.json AND package-lock.json are copied.
# Copying this separately prevents re-running npm install on every code change.
COPY package*.json ./

# Creates a non-root user to be used exclusively to run the application.
# Install production dependencies.
RUN groupadd -g $USER_GID $USER && \
    useradd -u $USER_UID -g $USER_GID -m $USER && \
    npm install --only=production

# Copy local code to the container image.
COPY . ./

# Set the non-root user as current.
USER $USER

# Run the web service on container startup.
CMD [ "npm", "start" ]
