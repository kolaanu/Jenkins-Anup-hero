# Use official Maven + JDK 17 as base
FROM maven:3.9.3-jdk-17

LABEL maintainer="Anup Kolambkar <kolambkaranup@gmail.com>"
ENV MAVEN_HOME=/usr/share/maven
ENV JAVA_HOME=/usr/local/openjdk-17

# Install Docker CLI, git, and other helpful utilities (ca-certificates, curl)
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    git \
    gnupg2 \
    lsb-release \
    apt-transport-https \
    && rm -rf /var/lib/apt/lists/*

# Install Docker CLI from Docker's repository (keeps only client)
RUN set -eux; \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg; \
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" \
      > /etc/apt/sources.list.d/docker.list; \
    apt-get update && apt-get install -y --no-install-recommends docker-ce-cli && rm -rf /var/lib/apt/lists/*

# Optional: create a non-root user (Jenkins may mount workspace and run as root or another user)
ARG USERNAME=jenkins
ARG UID=1000
RUN useradd -m -u ${UID} -s /bin/bash ${USERNAME} || true

# Ensure maven and java are on PATH (base image already sets them)
ENV PATH="${MAVEN_HOME}/bin:${PATH}"

# Default to non-root user to be safe (comment out if you need root)
USER ${USERNAME}

WORKDIR /home/${USERNAME}
