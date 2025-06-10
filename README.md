# AIConf 2025 Scheduler
![image](https://github.com/user-attachments/assets/783278f4-b433-49fd-bc54-340dd401e9d8)

www.aiconf-schedule.xyz

Un’app single-page che permette ai partecipanti di AIConf2025 di costruirsi un programma personalizzato in pochi click.

Agenda ufficiale: https://www.aiconf.it/e/3586/AI-Conf-2025

----------------

## 🚀 Panoramica

- **Frontend**: HTML/CSS/JS generati quasi del tutto via AI  
- **Storage & Hosting**: AWS S3 + CloudFront  
- **Dominio**: dominio personalizzato gestito via Route 53  
- **Autenticazione e sicurezza**: HTTPS, IAM least-privilege, Policy zero-trust
- **Persistenza locale**: `localStorage` per salvare il proprio schedule (nessun DB, dei tuoi dati non so nulla!)

-----------------

## 🏗 Architettura

![aiconf drawio](https://github.com/user-attachments/assets/fba8de02-2387-49b7-93c4-6096bc787659)


## 💼 Di cosa vado fiero

- **Architettura & Operazioni**: provisioning e hardening dei servizi AWS  
- **Ottimizzazione**: configurazione header HTTP e TTL cache  
- **Single-page solution**: nessun database, nessuna logica di autenticazione server-side — tutto è client-side per garantire semplicità, privacy e rapidità di deployment  

**Nota**: tutto il frontend e quasi l'intero codice è stata generato via AI; il mio contributo principale è stato il deploy in cloud e l’attenzione alla privacy e all’usabilità.

Scopri come ho fatto step by step all'interno dei docs!
