FROM ubuntu:latest

RUN apt-get update && apt-get install -y \
    curl \
    git \
    lsb-release \
    sudo \
    ca-certificates \
    gnupg \
    gnupg2 \
    software-properties-common \
    && rm -rf /var/lib/apt/lists/*

RUN curl -fsSL https://get.docker.com -o get-docker.sh && \
    sh get-docker.sh

RUN groupadd docker && usermod -aG docker $USER

RUN systemctl enable docker && systemctl start docker

CMD ["bash"]

