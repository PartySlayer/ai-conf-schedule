# Step by Step: Hosting static website su S3 + CloudFront

---

## 1. Creazione del bucket S3

Prima di tutto creiamo il bucket e inseriamo al suo interno i file necessari:

![Passo 1](media/image1.png)

Mi sono assicurato che l’accesso pubblico non fosse bloccato e ho aggiunto la policy:

![Accesso libero](media/image2.png)
![Policy di bucket](media/image3.png)

---

## 2. Abilitazione dell’hosting statico

A questo punto ho abilitato l’hosting, in modo da ottenere un endpoint:

![Hosting statico](media/image4.png)

http://www.aiconf-schedule.xyz.s3-website.eu-south-1.amazonaws.com/


![Hosting da bucket](media/image5.png)

---

## 3. Forzare HTTPS e redirect HTTP→HTTPS

Ho quindi forzato l’utilizzo di HTTPS e reindirizzato le richieste HTTP semplici:

![Forzatura HTTPS](media/image6.png)

Per fare questo ho creato una Hosted Zone su Route 53 in cui specificare il dominio che ho acquistato:

![Hosted Zone Route 53](media/image7.png)

Successivamente ho settato i nameserver forniti da Route 53 come custom DNS in Namecheap:

![Nameserver in Namecheap](media/image8.png)

---

## 4. Certificato SSL/TLS con ACM

Per usare HTTPS il trasporto dei dati deve avvenire attraverso SSL/TLS, abbiamo quindi bisogno di un certificato che attesti l’integrità del sito web.

Per questo ho utilizzato ACM (AWS Certificate Manager) per creare, validare e associare tramite DNS un certificato pubblico:

![ACM - richiesta certificato](media/image9.png)
![ACM - validazione DNS](media/image10.png)
![ACM - certificato ottenuto](media/image11.png)

---

## 5. Configurazione di CloudFront

Ho quindi creato una distribuzione CloudFront:

![Distribuzione CloudFront](media/image12.png)

Qui ho specificato di voler creare un record CNAME, inserendo il dominio acquistato e utilizzando il certificato pubblico precedentemente ottenuto. Così facendo la pagina web beneficerà di tutti i vantaggi di CloudFront, ma utilizzando il dominio personalizzato.

![Distribuzione CNAME](media/image13.png)

---

## 6. Record A in Route 53

Nella stessa Hosted Zone ho creato un record **A** per mappare `aiconf-schedule.xyz` alla distribuzione CloudFront:

![Record A Route 53](media/image14.png)
![Record A pending](media/image15.png)
![Record A in sync](media/image16.png)

Una volta che la propagazione DNS è avvenuta correttamente, è possibile visitare la pagina web in HTTPS utilizzando l’URL personalizzato:

![Sito HTTPS](media/image17.png)

---

## 7. Restrizione accesso diretto al bucket

Adesso però il bucket è ancora accessibile pubblicamente; voglio che il sito sia raggiungibile **solo** tramite CloudFront.

![Bucket ancora pubblico](media/image18.png)

1. Ho rimosso l’opzione di web hosting S3 e bloccato l’accesso pubblico.

![Blocca accesso pubblico](media/image19.png)

2. Ho modificato la policy in modo che solo la distribuzione CloudFront possa leggere dal bucket.

![Nuova bucket policy](media/image20.png)

Da questo momento l’accesso diretto al bucket S3 è negato:

![Accesso diretto negato](media/image22.png)

---

## 8. Origin Access Control (OAC)

Adesso bisogna modificare l’origine CloudFront (non più l’endpoint S3) tramite OAC:

![Creazione OAC](media/image23.png)
![Configurazione OAC](media/image24.png)

---

## 9. Impostare `index.html` come documento predefinito

Ora resta da specificare a CloudFront di servire il file `index.html` sul percorso root `/`.  
Modifichiamo le impostazioni generali della distribuzione:

![Index document](media/image25.png)

Ho impostato anche la reindirizzazione degli errori, così che se l'utente cerca un percorso inesistente viene riportato sulla pagina principale:

![Errori Custom](media/image26.png)

Anche senza l’opzione di web hosting di S3 è ora possibile accedere al sito, e solo tramite CloudFront (sia con l’URL della distribuzione, sia con il nostro alias personalizzato):

![Sito finale](media/image27.png)
