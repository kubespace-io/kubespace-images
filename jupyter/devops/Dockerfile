ARG IMAGE
FROM ${IMAGE}

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Berlin

USER root

# Install required packages in a single RUN command
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y gnupg2 curl gnupg software-properties-common \
                       unixodbc-dev postgresql libpq-dev default-jre \
                       ansible sshpass groff unzip

# Add Microsoft repository key
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

# Add Microsoft package repository
RUN curl https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/prod.list > /etc/apt/sources.list.d/msprod.list

# Install msodbcsql18 and mssql-tools
RUN apt-get update && \
    ACCEPT_EULA=Y apt-get install -y msodbcsql18 && \ 
    ACCEPT_EULA=Y apt-get install -y mssql-tools18

# Add HashiCorp repository for Terraform
RUN curl -fsSL https://apt.releases.hashicorp.com/gpg | apt-key add - && \
    apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main" && \
    apt-get update && \
    apt-get install -y terraform

# Install kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && \
    curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256" && \
    echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check && \
    install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl && \
    rm kubectl kubectl.sha256

# Install Azure CLI
RUN curl -sL https://aka.ms/InstallAzureCLIDeb | bash && \
    rm -rf /root/.azure

# Install AWS CLI
RUN curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
RUN unzip awscliv2.zip && \
    ./aws/install

# Install Napptive CLI
RUN curl -O https://storage.googleapis.com/artifacts.playground.napptive.dev/installer.sh && \
    bash installer.sh

COPY jupyter/devops/requirements.txt requirements.txt
RUN pip install --no-cache-dir -r requirements.txt && \
    rm requirements.txt

# Set 'kube' user as the active user
USER kube

# Set the working directory
WORKDIR /home/kube

# Expose port 8888 for accessing Jupyter Notebook
EXPOSE 8888

# Default command to start Jupyter Notebook
CMD ["jupyter-lab", "--ip=0.0.0.0", "--port=8888", "--no-browser"]

