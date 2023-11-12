# Copyright 2020 The Knative Authors
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM node:12-slim

ARG USER=knative
ARG USER_UID=1001
ARG USER_GID=$USER_UID
ARG ENV=production

# Doc port listening port
ENV PORT 8080
# Set current node env
ENV NODE_ENV $ENV

# Create and change to the app directory.
WORKDIR "/home/${USER}/app"

# Copy application dependency manifests to the container image.
# A wildcard is used to ensure both package.json AND package-lock.json are copied.
# Copying this separately prevents re-running npm install on every code change.
COPY package*.json ./

# Use ci is faster and more reliable following package-lock.json
RUN groupadd -g $USER_GID $USER && \
    useradd -u $USER_UID -g $USER_GID -m $USER && \
    npm ci --only=production

# Copy local code to the container image.
COPY . ./

EXPOSE $PORT

# Set the non-root user as current.
USER $USER

# Run the web service on container startup.
CMD npm run $NODE_ENV
