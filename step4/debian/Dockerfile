FROM debian:latest

RUN apt-get update && \
    apt-get install -y openssh-server sudo && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir /var/run/sshd && \
    sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config && \
    sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config && \
    sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config && \
    echo 'AllowUsers sshuser' >> /etc/ssh/sshd_config

RUN useradd -m sshuser

RUN mkdir -p /home/sshuser/.ssh && \
    chown sshuser:sshuser /home/sshuser/.ssh

RUN echo "sshuser ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/sshuser \
    && chmod 0440 /etc/sudoers.d/sshuser

COPY id_rsa.pub /home/sshuser/.ssh/authorized_keys

RUN chown sshuser:sshuser /home/sshuser/.ssh/authorized_keys && \
    chmod 700 /home/sshuser/.ssh && \
    chmod 600 /home/sshuser/.ssh/authorized_keys

RUN echo "root:{{ ssh_user_password }}" | chpasswd

ARG SSH_PASSWORD
RUN echo "root:$SSH_PASSWORD" | chpasswd

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
