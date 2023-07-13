# Start from the lightweight latest Ubuntu image
ARG IMAGE
FROM ${IMAGE}

# Switch to root user for system updates
USER root

# Install locales and tzdata
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y locales tzdata && \
    rm -rf /var/lib/apt/lists/* && \
    localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8 && \
    ln -fs /usr/share/zoneinfo/Europe/Berlin /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

# Set environment variables
ENV LANG en_US.utf8

# Install basic libraries
RUN apt-get update && \
    apt-get install -y curl wget && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create a non-root user
RUN useradd -m -s /bin/bash kube

USER kube
# Set the working directory to the 'kube' user's home directory
WORKDIR /home/kube

# Default command when starting the container
CMD ["/bin/bash"]
