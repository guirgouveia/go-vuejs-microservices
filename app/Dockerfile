# syntax=docker/dockerfile:1.6.0

# Demonstrates the use of ARG and ENV Dockerfile instructions
# to define the home directory of the application
# Reference: https://docs.docker.com/engine/reference/builder/#arg
# Reference: https://docs.docker.com/engine/reference/builder/#env
ARG APP_HOME=/app
ARG HTTP_PORT=8080

# Creates the build stage from the official Golang image
FROM golang:1.21.2 AS builder

ARG APP_HOME
ENV APP_HOME="$APP_HOME"

# Creates an "$APP_HOME" directory and changes the working directory to it
WORKDIR "$APP_HOME"

# Copies the go.mod and go.sum files to the "$APP_HOME" directory
COPY go.* ./

# Downloads the dependencies
RUN go mod download

# Verifies dependencies
RUN go mod verify

# Copy local code to the container image.
COPY . ./

# Builds the application
RUN go build -v -o main

# Creates a temporary folder with the files 
# to be published as an artifact to the next stage
RUN mkdir -p /tmp/output && \
    cp $APP_HOME/main /tmp/output && \
    cp $APP_HOME/server.confi /tmp/output && \
    cp -r $APP_HOME/src /tmp/output/src

# Creates the final stage from the official Debian slim image
FROM debian:stable-slim AS final

# Sets the ARGs and ENVs
ARG APP_HOME
ARG HTTP_PORT
ENV APP_HOME="$APP_HOME"
ENV HTTP_PORT="$HTTP_PORT"

# Set the default shell to /bin/sh
SHELL ["/bin/sh", "-c"]

# Set the working directory
WORKDIR /app

# Copies only the necessary files from the previous stage
COPY --from=builder /tmp/output/ ./

# Install ca-certificates for HTTPS
RUN set -x && apt-get update -y && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    ca-certificates

# Optimize image and install curl for healthcheck
# Install tzdata and locales for timezone and locale configuration
RUN set -x && apt-get update -y && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    tzdata \
    locales \
    curl && \
    rm -rf /var/lib/apt/lists/* && \
    locale-gen en_US.UTF-8 

# Set the timezone to UTC
ENV TZ=UTC

# Set the locale to en_US.UTF-8
ENV LANG=en_US.UTF-8

# Create a new group "stack-io" with GID 999
RUN addgroup --gid 999 stack-io

# Create a new user "stack-io" with UID 999 and add it to the newly created "stack-io" group
RUN adduser --uid 999 --ingroup stack-io --shell /bin/bash --disabled-password --gecos "" stack-io

# Creates the logs folder and assigns the "stack-io uid:gid" to it
RUN mkdir -p /var/logs/webserver

# Changes the owner of the logs folder to "stack-io"
RUN chown -R 999:999 /var/logs/webserver

# Create healthcheck 
HEALTHCHECK --interval=5s \
            --timeout=3s \
            --retries=3 \
            CMD curl -f http://localhost:"$HTTP_PORT"/health || exit 1

# Switch to the new user
USER stack-io

# Exposes port 8080
EXPOSE "$HTTP_PORT"
    
# Runs the application
CMD "$APP_HOME/main"