##########################################
# parameters
##########################################

# [Choice] Ubuntu version: bionic, focal
ARG VARIANT="focal"

##########################################
# base image
##########################################

# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.154.0/containers/ubuntu/.devcontainer/base.Dockerfile
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

##########################################
# users
##########################################

RUN groupadd -r containergroup && useradd -r -g containergroup --create-home containeruser

##########################################
# context
##########################################

WORKDIR /devcontainer/docker-build-workdir
USER root

##########################################
# repository setup
##########################################

# microsoft
RUN sudo apt-get update \
    && sudo apt-get install -y wget apt-transport-https software-properties-common \
    && wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb \
    && sudo dpkg -i packages-microsoft-prod.deb 

# hashicorp
RUN curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add - \
    && sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"

# RUN sudo add-apt-repository universe
RUN sudo apt-get update 

##########################################
# powershell
##########################################

RUN sudo apt-get install -y powershell \
    && pwsh --version

RUN pwsh -noni -nol -nop -c Install-Module PSFzf -Scope AllUsers -Force

SHELL ["/usr/bin/pwsh", "-nop", "-nol", "-c"]

##########################################
# core utilities
##########################################

RUN sudo apt-get install -y file

RUN sudo apt-get install -y tmux vim ranger fzf \
    && tmux -V  \
    && fzf --version

##########################################
# network utilities
##########################################

RUN sudo apt install -y iputils-ping dnsutils socat ipcalc
RUN echo "wireshark-common wireshark-common/install-setuid boolean true" | sudo debconf-set-selections
RUN sudo apt install -y wireshark tshark termshark

##########################################
# linuxbrew
##########################################

# RUN git clone https://github.com/Homebrew/brew ~/.linuxbrew/Homebrew \
#     && mkdir ~/.linuxbrew/bin \
#     && ln -s ~/.linuxbrew/Homebrew/bin/brew ~/.linuxbrew/bin \
#     && ln -s ~/.linuxbrew/Homebrew/bin/brew /usr/local/bin/ \
#     && /bin/bash -c eval $(~/.linuxbrew/bin/brew shellenv) \
#     && brew update

##########################################
# npm
##########################################

RUN sudo apt install -y npm \
    && npm version

RUN npm cache clean -f \
    && npm install -g n \
    && n stable

RUN npm install --global yarn \
    && yarn --version

##########################################
# one password
##########################################

RUN iwr https://cache.agilebits.com/dist/1P/op/pkg/v1.12.3/op_linux_amd64_v1.12.3.zip -o op.zip \
    && unzip op.zip \
    && mv op /usr/local/bin/op \
    && op --version

##########################################
# step
##########################################

RUN iwr https://dl.step.sm/gh-release/cli/gh-release-header/v0.18.0/step_linux_0.18.0_amd64.tar.gz -o step.tar.gz \
    && tar xzf step.tar.gz  \
    && chmod +x step_0.18.0/bin/step \
    && mv step_0.18.0/bin/step /usr/local/bin/step \
    && step version

##########################################
# docker
##########################################

RUN sudo apt-get install -y docker.io \
    && docker --version

RUN iwr "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-Linux-x86_64" -outfile /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose \
    && docker-compose version

RUN usermod -aG docker containeruser
RUN usermod -aG docker vscode

##########################################
# github
##########################################

RUN iwr "https://github.com/cli/cli/releases/download/v2.0.0/gh_2.0.0_linux_amd64.deb" -outfile gh.deb \
    && dpkg -i gh.deb \
    && gh version

##########################################
# rust
##########################################

RUN curl https://sh.rustup.rs -sSf | sh -s -- -y

##########################################
# dotnet
##########################################

RUN sudo apt-get install -y dotnet-sdk-7.0 \
    && dotnet --version

##########################################
# java
##########################################

RUN sudo apt-get install -y openjdk-8-jdk \
    && java -version \
    && javac -version

##########################################
# python
##########################################

RUN sudo apt install -y python3-pip python3.8
RUN python3 --version \
    && pip3 --version

RUN pip3 install virtualenv black pandas numpy

##########################################
# typescript
##########################################

RUN sudo npm install -g typescript \
  && tsc --version

##########################################
# aws
##########################################

RUN iwr "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -outfile "awscliv2.zip" \
    && unzip awscliv2.zip \
    && sudo ./aws/install \
    && rm -rf aws* \
    && aws --version

RUN sudo pwsh -c Install-Module -Scope AllUsers -Name AWSPowerShell.NetCore -Force

##########################################
# vault
##########################################

RUN sudo apt-get install -y vault \
    && sudo apt-get install --reinstall -y vault \
    && vault version

##########################################
# terraform
##########################################

RUN iwr "https://releases.hashicorp.com/terraform/1.0.8/terraform_1.0.8_linux_amd64.zip" -outfile "terraform.zip" \
    && unzip terraform.zip \
    && mv terraform /usr/local/bin/terraform \
    && rm terraform.zip \
    && terraform version

##########################################
# redis
##########################################

RUN apt install -y redis-tools \
    && redis-cli --version

##########################################
# kubernetes
##########################################

# kubectl
RUN iwr "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl" -outfile kubectl \
    && chmod +x kubectl \
    && sudo mv ./kubectl /usr/local/bin/kubectl \
    && kubectl version --client

# k9s
RUN iwr "https://github.com/derailed/k9s/releases/download/v0.24.15/k9s_Linux_x86_64.tar.gz" -outfile k9s.tar.gz \
    && tar xzf k9s.tar.gz k9s \
    && mv k9s /usr/local/bin/k9s \
    && k9s version

# kubectx
RUN iwr "https://github.com/ahmetb/kubectx/releases/download/v0.9.3/kubectx_v0.9.3_linux_x86_64.tar.gz" -outfile kubectx.tar.gz \
    && tar xzf kubectx.tar.gz kubectx \
    && mv kubectx /usr/local/bin/kubectx \
    && kubectx --help

# kubens
RUN iwr "https://github.com/ahmetb/kubectx/releases/download/v0.9.3/kubens_v0.9.3_linux_x86_64.tar.gz" -outfile kubens.tar.gz \
    && tar xzf kubens.tar.gz kubens \
    && mv kubens /usr/local/bin/kubens \
    && kubens --help

# kustomize
RUN iwr "https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2Fv4.1.2/kustomize_v4.1.2_linux_amd64.tar.gz" -outfile kustomize.tar.gz \
    && tar xzf kustomize.tar.gz kustomize \
    && mv kustomize /usr/local/bin/kustomize \
    && kustomize version

##########################################
# helm
##########################################

RUN iwr "https://get.helm.sh/helm-v3.7.0-linux-amd64.tar.gz" -outfile helm.tar.gz \
    && tar xzf helm.tar.gz linux-amd64/helm \
    && mv linux-amd64/helm /usr/local/bin/helm \
    && helm version

##########################################
# sealed secrets
##########################################

# kubeseal
RUN iwr "https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.17.1/kubeseal-0.17.1-linux-amd64.tar.gz" -outfile kubeseal.tar.gz \
    && tar xzf kubeseal.tar.gz kubeseal \
    && chmod +x ./kubeseal \
    && mv kubeseal /usr/local/bin/kubeseal \
    && kubeseal --version

##########################################
# argo
##########################################

# argo cd
RUN iwr "https://github.com/argoproj/argo-cd/releases/download/v2.1.3/argocd-linux-amd64" -o argocd \
    && chmod +x ./argocd \
    && mv ./argocd /usr/local/bin/argocd \
    && argocd version --client

# argo workflows
RUN iwr "https://github.com/argoproj/argo-workflows/releases/download/v3.1.13/argo-linux-amd64.gz" -outfile argo-linux-amd64.gz \
    && gunzip argo-linux-amd64.gz \
    && chmod +x argo-linux-amd64 \
    && mv ./argo-linux-amd64 /usr/local/bin/argo \
    && argo version

##########################################
# cert-manager
##########################################

RUN iwr https://github.com/jetstack/cert-manager/releases/download/v1.7.0-alpha.0/cmctl-linux-amd64.tar.gz -outfile cmctl.tar.gz \
    && tar xzf cmctl.tar.gz cmctl \
    && chmod +x cmctl \
    && mv ./cmctl /usr/local/bin/cmctl \
    && cmctl version --client

##########################################
# linkerd
##########################################

RUN iwr https://github.com/linkerd/linkerd2/releases/download/stable-2.11.1/linkerd2-cli-stable-2.11.1-linux-amd64 -outfile linkerd \
    && chmod +x linkerd \
    && mv ./linkerd /usr/local/bin/linkerd \
    && linkerd version --client

##########################################
# istio
##########################################

RUN iwr https://github.com/istio/istio/releases/download/1.12.1/istioctl-1.12.1-linux-amd64.tar.gz -outfile istioctl.tar.gz \
    && tar xzf istioctl.tar.gz istioctl \
    && chmod +x istioctl \
    && mv ./istioctl /usr/local/bin/istioctl \
    && istioctl version --remote=false

##########################################
# vscode
##########################################

# RUN npm install -g vsce \
#   && vsce --version

##########################################
# scripts
##########################################

WORKDIR /devcontainer/docker-context
COPY /postCreate.ps1 ./

##########################################
# user
##########################################

USER root

##########################################
# entrypoint 
##########################################

ENTRYPOINT [ "/bin/pwsh", "-nop", "-nol", "-c"]
CMD [ "while ($true) {start-sleep 30}" ]
WORKDIR /
