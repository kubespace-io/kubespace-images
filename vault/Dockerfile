ARG IMAGE
FROM ${IMAGE}

USER root
WORKDIR /

RUN apt-get update && apt-get install -y gpg unzip
RUN wget https://releases.hashicorp.com/vault/1.14.0-rc1/vault_1.14.0-rc1_linux_amd64.zip && \
    unzip vault_1.14.0-rc1_linux_amd64.zip -d /usr/local/bin/ && \
    rm vault_1.14.0-rc1_linux_amd64.zip && \
    apt-get remove -y unzip && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

EXPOSE 8200
EXPOSE 8201

USER kube
WORKDIR /home/kube

COPY vault/config.hcl config.hcl
COPY vault/start-vault.sh start-vault.sh

CMD ["sh","start-vault.sh"]