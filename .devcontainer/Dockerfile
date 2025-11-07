# Use Debian bookworm (latest stable) slim for smaller size
FROM debian:bookworm-slim

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies and development tools
RUN apt-get update && apt-get install -y \
    # Build essentials
    build-essential \
    pkg-config \
    curl \
    git \
    # OpenSSL development libraries
    libssl-dev \
    # ALSA development libraries (for audio support)
    libasound2-dev \
    # Additional useful tools
    vim \
    nano \
    less \
    wget \
    ca-certificates \
    # For debugging
    gdb \
    lldb \
    # Sudo for the vscode user
    sudo \
    && rm -rf /var/lib/apt/lists/*

# Create a non-root user with sudo access
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Switch to non-root user
USER $USERNAME
WORKDIR /home/$USERNAME

# Install Rust as the vscode user
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y \
    && . $HOME/.cargo/env \
    && rustup component add clippy rustfmt rust-src

# Add cargo to PATH
ENV PATH="/home/$USERNAME/.cargo/bin:${PATH}"

# Verify installations
RUN rustc --version && cargo --version

# Set the default shell to bash
SHELL ["/bin/bash", "-c"]

# Switch back to dialog for any ad-hoc use
ENV DEBIAN_FRONTEND=dialog

# Default command
CMD ["/bin/bash"]
