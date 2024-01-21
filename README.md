# Use Ubuntu 20.04 LTS as the base image
FROM ubuntu:20.04

# Avoid prompts from apt
ENV DEBIAN_FRONTEND=noninteractive

# Update and install ubuntu-desktop, VNC, SSH, and other necessary tools
RUN apt-get update && apt-get install -y \
    ubuntu-desktop \
    tightvncserver \
    ssh \
    git \
    curl \
    wget \
    vim \
    build-essential \
    # Add additional dev tools here (up to 25 as per your requirement)
    && rm -rf /var/lib/apt/lists/*

# Set up VNC
RUN mkdir /root/.vnc
# Set VNC password to 'password', change as needed
RUN echo 'password' | vncpasswd -f > /root/.vnc/passwd
RUN chmod 600 /root/.vnc/passwd

# Install Node.js and NVM
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
ENV NVM_DIR=/root/.nvm
RUN . "$NVM_DIR/nvm.sh" && nvm install node # Install latest Node.js version

# Expose VNC and SSH ports
EXPOSE 5901 22

# Clone the GitHub repository and serve it on port 8080
# Replace 'your-github-repo-link' with the actual GitHub repository link
RUN git clone your-github-repo-link /app
WORKDIR /app
# Assuming the GitHub repo has a setup to serve on port 8080
# Add specific commands here as required

# Start VNC server
CMD ["vncserver", "-geometry", "1280x800", "-depth", "24", "-localhost", "no", ":1"]
