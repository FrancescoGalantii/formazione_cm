### **`obiettivo`**
creare un container che abbia attivo al suo interno il servizio docker(docker in docker) e una pipeline jenkins che esegua il build dell'immagine, la tagghi in modo progressivo e effettui il push sul registry.

# Spiegazione Pipeline Jenkins 

*`La pipeline gestisce il ciclo di vita di build, tagging e push di un'immagine Docker in un registro Docker privato`*.

## Funzionalità della Pipeline

La pipeline è strutturata per eseguire i seguenti passaggi:

• **`Checkout del codice sorgente`**

• **`Build e tagging di un'immagine Docker`**

• **`Push dell'immagine Docker al registro Docker privato`**

• **`Pulizia dell'ambiente di lavoro alla fine`**

## Descrizione della Pipeline

### Ambiente

La pipeline utilizza le seguenti variabili di ambiente:

• `REGISTRY`: Specifica l'indirizzo del registro Docker (default: `localhost:5000`).

• `IMAGE_NAME`: Nome base dell'immagine Docker (default: `mystep5`).

• `TAG`: Tag di default per l'immagine Docker (default: `latest`).

### Stages

#### 1. Checkout

• **Scopo:** Effettua il clone del repository

    stage('Checkout') {
        steps {
            checkout scm
        }
    }

#### 2. Build e tagging di un'immagine Docker

  • Determina il tag dinamicamente utilizzando `build-${env.BUILD_NUMBER}`.  
  
  • Costruisce l'immagine usando il comando `docker.build(imageTag, '.')`.
  
  • Assegna il tag generato alla variabile di ambiente `IMAGE_TAG` per uso successivo.

#### 3. Push dell'immagine Docker

• **Scopo:** Esegue il push dell'immagine Docker al registro Docker configurato, nel mio caso ho ripreso un registry privato locale.

• **Dettagli:**

  • Utilizza il registro Docker specificato nella variabile `REGISTRY`.
  
  • Effettua il login temporaneo al registro utilizzando `docker.withRegistry`.
  
  • Esegue il push dell'immagine utilizzando il comando `image.push()`.

### Post 

    post {
        always {
            cleanWs() 
        }
    }
La sezione `post` garantisce che l'ambiente di lavoro venga pulito indipendentemente dal risultato della pipeline (successo, errore o interruzione).

## Come configurare e utilizzare la Pipeline

1. **Prerequisiti:**
   • Installare Jenkins con il plugin Docker abilitato.
   
   • Configurare un registro Docker accessibile all'indirizzo `localhost:5000` (o modificare il valore della variabile `REGISTRY`).

3. **Passaggi:**
   • Salvare il file Jenkinsfile nel repository del progetto.
   
   • Configurare un job Jenkins che punti al repository del progetto.
   
   • Assicurarsi che Docker sia installato e configurato sul server Jenkins.

5. **Esecuzione:**
   • Avviare la pipeline da Jenkins.
   
   • Monitorare il processo nei log di Jenkins per verificare che il build e il push dell'immagine siano completati con successo.

## Note aggiuntive

• **Modifica del registro Docker:** Se si utilizza un registro Docker diverso, modificare il valore della variabile `REGISTRY` nella sezione `environment`.

• **Gestione degli errori:** Assicurarsi che Jenkins abbia i permessi necessari per accedere al registro Docker.

• **Pulizia automatica:** La pulizia finale garantisce che lo spazio su disco del server Jenkins venga ottimizzato.

