# Copyright 2018 The Knative Authors
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

# Use the official Golang image to create a build artifact.
# This is based on Debian and sets the GOPATH to /go.
# https://hub.docker.com/_/golang
FROM golang AS builder

ARG TARGETOS
ARG TARGETARCH

# Create and change to the app directory.
WORKDIR /app

# Copy local code to the container image.
COPY . ./

RUN go mod tidy

# Build the command inside the container.
# To facilitate gRPC testing, this container includes a client command.
RUN CGO_ENABLED=0 GOOS=${TARGETOS} GOARCH=${TARGETARCH} go build -tags=grpcping -o ./ping-server
RUN CGO_ENABLED=0 GOOS=${TARGETOS} GOARCH=${TARGETARCH} go build -tags=grpcping -o ./ping-client ./client

# Use a Docker multi-stage build to create a lean production image.
# https://docs.docker.com/develop/develop-images/multistage-build/#use-multi-stage-builds
# https://github.com/GoogleContainerTools/distroless#readme
FROM gcr.io/distroless/static

# Copy the binaries to the production image from the builder stage.
COPY --from=builder /app/ping-server /server
COPY --from=builder /app/ping-client /client

# Run the service on container startup.
CMD ["/server"]
