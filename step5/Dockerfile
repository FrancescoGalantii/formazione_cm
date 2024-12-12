FROM ubuntu:latest

WORKDIR /app

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y \
    curl \
    vim \
    git \
    && rm -rf /var/lib/apt/lists/*

CMD ["echo", "Hello, World! This is your Ubuntu container."]

