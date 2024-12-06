### **`obiettivo`**
Utilizzare i playbook degli step precedenti come task per creare dei ruoli che avessero le seguenti caratteristiche:

• `Creazione e configurazione di un registry`

• `Build di almeno due container`

• `Push delle build sul regristy precedentemente creato`

• `Run dei container in modo che non vadano in conflitto di porte tra loro`

• `funzionare sia con Docker che con Podman`

**`struttura`**

    project
    ├── debian
    │   ├── Dockerfile
    │   └── id_rsa.pub
    ├── playbook.yml
    ├── roles
    │   ├── build_and_run_containers
    │   │   └── tasks
    │   │       └── main.yml
    │   └── registry
    │       └── tasks
    │           └── main.yml
    └── ubuntu
        ├── Dockerfile
        └── id_rsa.pub
All'interno dei file main.yml ho definito i task che poi sono andato a richiamare nel playbook principale in questo modo:

    roles:
      - registry
      - build_and_run_containers
