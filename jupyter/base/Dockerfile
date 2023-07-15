ARG IMAGE
FROM ${IMAGE}

USER root

# Install Python and pip, and cleanup
RUN apt-get update && \
    apt-get install -y python3 python3-pip && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install Jupyter Notebook
RUN pip3 install jupyterlab

# Set 'kube' user as the active user
USER kube

# Set the working directory
WORKDIR /home/kube

# Expose port 8888 for accessing Jupyter Notebook
EXPOSE 8888

# Default command to start Jupyter Notebook
CMD ["jupyter-lab", "--ip=0.0.0.0", "--port=8888", "--no-browser"]