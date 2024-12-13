### **`obiettivo`**
Utilizzare ansible vault per oscurare tutte le password.

## `spiegazione procedimenti`

Creare un file `vault.yml` con all'interno le password da utilizzare e oscurare.

## `modifiche playbook`

All'interno del playbook ho aggiunto il file vault tramite attraverso `vars_files` , *direttiva utilizzata per includere file di variabili estern*e.

    vars_files:
      - vault.yml
## `modifiche directory roles`

All'interno della directory roles sono presenti altre due directory contenenti tutte le task eseguite finora la struttura è la seguente:

    roles
    ├── build_and_run_containers
    │   └── tasks
    │       └── main.yml
    └── registry
        └── tasks
            └── main.yml

    5 directories, 2 files
Ho modificato entrambi i main.yml aggiungendo l'uso delle password specificate all'interno del vault

## `verifiche finali`

• Verificare innanzitutto se i container e il docker registry sono stati ricreati correttamente lanciare quindi il comando `docker ps`
se ricreati correttamente l'output risulterà questo:

    CONTAINER ID   IMAGE               COMMAND                  CREATED       STATUS       PORTS                    NAMES
        ID         ssh_debian:latest   "/usr/sbin/sshd -D"      2 hours ago   Up 2 hours   0.0.0.0:2202->22/tcp     container_debian
        ID         ssh_ubuntu:latest   "/usr/sbin/sshd -D"      2 hours ago   Up 2 hours   0.0.0.0:2201->22/tcp     container_ubuntu
        ID         registry:2          "/entrypoint.sh /etc…"   2 days ago    Up 2 hours   0.0.0.0:5000->5000/tcp   docker-registry
    
• lanciare il comando lanciato sullo step3 per accedere al container con l'utente generato ma sta volta inserendo la password specificata nel vault:

`ssh -i /Users/francescogalanti/.ssh/id_rsa -p 2201 sshuser@localhost` 

