# Use Node.js 23.6 as base image
FROM node:23.6

# Create a non-root user for GitHub Codespaces
RUN useradd -m -s /bin/bash codespace

# Install Python and uv for uvx
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    python3-venv \
    curl \
    git \
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Add codespace user to sudo group
RUN usermod -aG sudo codespace && \
    echo "codespace ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Switch to codespace user
USER codespace

# Install uv (which provides uvx) for the codespace user
RUN curl -LsSf https://astral.sh/uv/install.sh | sh

# Add uv to PATH for codespace user
ENV PATH="/home/codespace/.local/bin:$PATH"

# Set working directory
WORKDIR /workspaces/sample-mcp-server-for-codespaces

# Install any additional global npm packages if needed
# RUN npm install -g <package-name>

# Copy package files (if they exist)
COPY package*.json ./
RUN if [ -f package.json ]; then npm install; fi

# Copy the rest of the application
COPY . .

# Expose common development ports
EXPOSE 3000 5000 8000

# Default command
CMD ["bash"]