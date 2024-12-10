FROM ubuntu:latest

RUN apt-get update && apt-get install -y \
    openssh-server \
    docker.io \
    sudo && \
    mkdir /var/run/sshd


RUN useradd -ms /bin/bash sshuser && \
    echo "sshuser:password" | chpasswd && \
    usermod -aG sudo sshuser 

RUN mkdir /home/sshuser/.ssh && \
    chmod 700 /home/sshuser/.ssh

COPY id_rsa.pub /home/sshuser/.ssh/authorized_keys
RUN chmod 600 /home/sshuser/.ssh/authorized_keys && \
    chown -R sshuser:sshuser /home/sshuser/.ssh

RUN sed -i 's/#Port 22/Port 22/' /etc/ssh/sshd_config && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config && \
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config


EXPOSE 22

CMD ["bash", "-c", "dockerd & sleep 5 && /usr/sbin/sshd -D"]
