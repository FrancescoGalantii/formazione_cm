 ### *`obiettivo`*
creare un docker registry anche senzq autenticazione tramite playbook.
In questo caso ho utilizzato il localhost importante però controllare prima se sull'host sia scaricato docker in caso contrario aggiungere task per l'installazione di docker sul playbook.

**`spiegazione playbook`**

1)**`Ho creato una directory per il docker registry`**

    - name: create a directory for the registry
      file:
        path: /Var/lib/registry
        state: directory
2)**`ho startato il docker registry assegnandogli porta e volume`**

    - name: start the registry without authentication
      docker_container:
        name: docker-registry
        image: registry:2
        state: started
        restart_policy: always
        ports:
          - "5000:5000"
        volumes: 
          - /var/lib/registry:/var/lib/registry
Per eseguire il playbook lanciare il seguente comando:

`ansible-playbook -i inventory container-playbook.yml --ask-become-pass`

L'opzione --ask-become-pass poichè sul playbook è settato become: true --> ossia `utenza root`

!! ATTENZIONE **`se non messa questa opzione all'esecuzione del playbook verrà restituito il seguente errore:
"module_stderr": "sudo: a password is required\n"`**



