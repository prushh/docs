# Use the official lightweight Python image.
# https://hub.docker.com/_/python
FROM python:3.7-slim

ARG USER=knative
ARG USER_UID=1001
ARG USER_GID=$USER_UID

ENV PORT=8080
# Allow statements and log messages to immediately appear in the Knative logs
ENV PYTHONUNBUFFERED=True
# Copy local code to the container image.
ENV APP_HOME=app

# Create and change to the app directory.
WORKDIR "/home/${USER}/${APP_HOME}"

COPY . ./

# Install production dependencies.
RUN groupadd -g $USER_GID $USER && \
    useradd -u $USER_UID -g $USER_GID -m $USER && \
    pip install Flask gunicorn

# Set the non-root user as current.
USER $USER

# Run the web service on container startup. Here we use the gunicorn
# webserver, with one worker process and 8 threads.
# For environments with multiple CPU cores, increase the number of workers
# to be equal to the cores available.
CMD exec gunicorn --bind :$PORT --workers 1 --threads 8 --timeout 0 app:app
