# RELAZIONE ESERCITAZIONE: Lab_Finale 
### DI ADAM

#### PICOLLA PREMESSA: 
Durante il lab ho riscontrato vari errori che non sono riuscito a documentare con screenshot, cercherò di menzionarli passo dopo passo.

### TROVERETE IN QUESTA REPOSITORY ANCHE LA CONSEGNA INTERA DEL LAB





#### 1 STEP


## Creazione risorse Azure tramite ambiente Terraform

Ho creato l'ambiente Terraform con la cartella main.tf, e variables.tf 

<img width="461" alt="SCREEN MAIN PER RELZIONE" src="https://github.com/user-attachments/assets/4406981c-b715-4a2c-af83-633cd98c7539" />



<img width="292" alt="SCREEN VARIABLES PROGETTO" src="https://github.com/user-attachments/assets/475948f8-5d0d-4269-b4cc-91aa92278cec" />


Una volta che ho creato correttamente i file main.tf e variables.tf, necessari per creare l'infrastrttura Azure come da consegna: Virtual Machine, Vnet e i gruppi di sicurezza per gestire il traffico di rete.

Mi sono loggato con il comando Az Login




<img width="362" alt="login azure" src="https://github.com/user-attachments/assets/30c8706b-d19d-4b8e-98fc-b12afd234674" />











<img width="710" alt="sottoscrizione" src="https://github.com/user-attachments/assets/0d13111d-2b9c-4d22-bc61-85437b62bb29" />






Adesso dobbiamo preparare Terraform per creare risorse. con il "Terraform init" scarica i programmi necessari per lavorare con Azure e sistema tutto per iniziare."

Prima di creare le risorse, possiamo vedere cosa verrà creato da Terraform con il comando : "Terraform paln"

Ultimo passaggio per applicare le configurazioni di Terraform e creare tutte le risorse in Azure eseguiamo il comando: "terraform apply"





### In questo momento avremo creato tutte le nostre risorse






<img width="955" alt="relazione" src="https://github.com/user-attachments/assets/26b9601b-1f1e-4f84-98b8-0edbe9c1edee" />














<img width="953" alt="risorse azure" src="https://github.com/user-attachments/assets/25d5f9fb-fa4e-456e-8d46-48a5333eb50b" />


### Errori riscontrati: 2
Inizialmente non riuscivo a creare le risorse per un errore con le Variabili, secondo errore anche se non lo definirei errore ho messo una size alla vm molto bassa (ram 0.5), avevo fatto questa scelta per poter risparmiare soldi poichè dovrò pagare il consumo delle risorse di tasca mia, quindi ho cambiato size perchè la vm era troppo lenta.













### STEP 2


## Configurazione del del Cluster K3s:

Accedo alla Vm con chiave ssh:


<img width="559" alt="image" src="https://github.com/user-attachments/assets/046b4821-a2f5-450c-bbc0-4463d53b1933" />



Ora installo Docker sulla Vm per poter containerizzare l'applicazione, con il comando: "curl -fsSL https://get.docker.com | sudo bash"




Per installare K3s (una versione leggera di Kubernetes), eseguiamo: "
curl -sfL https://get.k3s.io | sh -"


dopodichè lanciamo il comando "sudo kubectl get nodes" per verificare che sia in esecuzione



Creiamo una nuova cartella per il nostro progetto Docker. La chiamiamo hello-docker
comando: "mkdir hello-docker"



vado nella directory hello docker
comando: "cd hello-docker"

creiamo un file di nome app.js che conterra il codice per l'applicazione Node.js
creiamo anche un file packaje.json per definire le dipendenze del progetto

comando per la creazione del file app.js: "nano app.js"



<img width="959" alt="relazione 5" src="https://github.com/user-attachments/assets/b6901e55-bd9e-4c27-88ca-7b9addec037c" />










comando per la creazione del file package.json: "nano package.json"


dopodichè dobbiamo installare le dipendenze, e le librerie con i comandi: "sudo apt install" e "npm install"


adesso è il momento di creare il dockerfile con il comando : " nano Dockerfile"









<img width="952" alt="screen dockerfile" src="https://github.com/user-attachments/assets/0fe55aee-e8cd-4c34-9ce7-2bbcf241d413" />




ora dobbiamo costruire l'immagine con il comando : " sudo docker build -t hello-docker . "

ora verifico l'immagine creata con il comando : " sudo docker images "





<img width="425" alt="image" src="https://github.com/user-attachments/assets/90054c11-b952-4370-aad8-41ade74f866d" />









### Erorri riscontrati: 1

Inizialmente con il comando "sudo docker images" non riuscivo a visualizzare l'immagine " localhost:500/hello-docker"
infatti quando lanciavo il comando successivo "sudo kubectl get pods" che approfondiremo nello step 3 mi dava che i pods non fossero ready, facendo un po' di troubleshoting mi sono accorto che kunernetes non riusciva a scaricare l'immagine del contaier specifica dei miei pod.





<img width="959" alt="relazione 9" src="https://github.com/user-attachments/assets/5ab0dd7a-6d27-45d9-8e1d-13a0e37e9673" />












<img width="959" alt="relazione 11" src="https://github.com/user-attachments/assets/6c91bd98-eff3-4791-9fd2-7c50206879df" />


ho risolto il problema ricreando l'immagine con il comando: "docker push localhost:500/hello-docker:latest"






<img width="958" alt="relazione 12" src="https://github.com/user-attachments/assets/53473f6e-b661-48f8-aef1-09b9c61e9691" />









### STEP 3



## Deployment dell'app



Ho creato un file Yaml per il deployment con comando: "nano deployment.yaml"







<img width="491" alt="image" src="https://github.com/user-attachments/assets/4a68bd70-33d1-4ce5-adce-36fd0ba42240" />





ho creato anche un file per i servizi chiamato service.yaml utilizzando il comando: "nano service.yaml"









<img width="437" alt="image" src="https://github.com/user-attachments/assets/5e26fd63-f94d-4f68-8049-95b8a20fef19" />








successivamente ho eseguito i comandi "sudo kubectl apply -f deployment.yaml" e "sudo kubectl apply -f service.yaml"
per creare il deployment e il servizio


dopodichè ho lanciato il comando "sudo kubectl get pods" per verificare lo stato dei pods



<img width="394" alt="image" src="https://github.com/user-attachments/assets/43a8c3a8-4e92-4f4d-9c95-51ff5e221d83" />






Ora tocca verificare che tutto funzioni con browser, utilizzando l'Ip pubblico e la porta 30080


il link è il seguente: http://13.81.104.106:30080/





<img width="959" alt="relazione 14" src="https://github.com/user-attachments/assets/834ec483-4d4d-4abe-b155-d5c57b9109dc" />



