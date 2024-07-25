# Use an Ubuntu-based image from Microsoft for compatibility with VS Code Dev Containers
FROM mcr.microsoft.com/vscode/devcontainers/base:bullseye

# Install necessary packages
RUN apt-get update && apt-get install -y \
    libglib2.0-0 \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Set up non-root user
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create necessary directories and set up the GitHub Actions runner
RUN mkdir -p /home/${USERNAME}/actions-runner
WORKDIR /home/${USERNAME}/actions-runner

# Input GitHub runner version argument
ARG RUNNER_VERSION=2.303.0

# Download, extract, and install dependencies for GitHub Actions runner
RUN curl -O -L https://github.com/actions/runner/releases/download/v${RUNNER_VERSION}/actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz \
    && tar xzf actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz \
    && ./bin/installdependencies.sh

# Copy the start.sh script into the container
COPY scripts/start.sh /home/${USERNAME}/actions-runner/start.sh

# Make the script executable and ensure proper permissions
RUN chown -R ${USERNAME}:${USERNAME} /home/${USERNAME} \
    && chmod +x /home/${USERNAME}/actions-runner/start.sh

# Set the user to non-root
USER ${USERNAME}

# Start the GitHub Actions runner
CMD ["/home/vscode/actions-runner/start.sh"]
