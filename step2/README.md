### **`obiettivo`**
Creare un playbook che esegue la build di container con OS differenti.
## spiegazione playbook

1)`costruire l'immagine docker per OS ubuntu tramite il modulo di ansible community.docker.docker_image`

    - name: Costruire l'immagine Docker per Ubuntu
      community.docker.docker_image:
        source: build
        build:
          path: ./ubuntu
        name: ssh_ubuntu
        tag: latest
per poi avviarlo in questo modo:

    - name: Avviare il container Ubuntu
      community.docker.docker_container:
        name: container_ubuntu
        image: ssh_ubuntu:latest
        state: started
        ports:
          - "2201:22"
        restart_policy: always
2)`ripetere lo stesso procedimento per il container con OS rockylinux`.
## spiegazione Dockerfile

1)`definire l'immagini` nel mio caso --> `FROM ubuntu:latest` per ubuntu e `FROM debian:latest` per debian 

2)`installare openssh, sudo ed ho creare la directory sshd`

    RUN apt-get update && apt-get install -y \
        openssh-server \
        sudo && \
        mkdir /var/run/sshd
3)`aggiungere l'utente sshuser`

    RUN useradd -ms /bin/bash sshuser && \
4)`creare ed assegnare i permessi di read, write e execution all'utente sshuser creato in precedenza`

    RUN mkdir /home/sshuser/.ssh && \
        chmod 700 /home/sshuser/.ssh
!!IMPORTANTE ai fini dell'esercizio per far si che il container sia sempre in ascolto sulla porta 22 

    EXPOSE 22
## servizio ssh

Sul mio localhost avevo gia create le chiavi ssh, ho di conseguenza fatto le seguenti operazioni:

`cd .ssh`: per accedere alla cartella contenente il file id_rsa.pub 

`cat id_rsa.pub`: per visualizzare in output la chiave pubblica e copiarla 

`cd id_rsa.pub`: che non è il file precedente ma un altro file creato all'interno delle relative directory dei container e dove all'interno delle quali ho incollato la public key copiata precedentemente.
## verifica funzionamento

Per poter verificare il corretto funzionamento dell' esercizio lanciare il seguente comando:

`ssh -i /Users/francescogalanti/.ssh/id_rsa -p 2201 sshuser@localhost`

• se la connessione ssh funziona --> `sshuser@0ae9b6346055:~$`






