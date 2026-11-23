---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_l.pdf
argomento: Servizi distribuiti Unix/Linux (NIS, NFS, Automount) e gestione di rete (SNMP)
tags:
  - reti
  - nis
  - nfs
  - automount
  - snmp
  - amministrazione-rete
---

# 🗂️ Servizi Distribuiti (NIS, NFS, Automount) e Gestione (SNMP)

## 1. [[NIS]] (*Network Information Service*)
Originariamente sviluppato da Sun Microsystems con il nome di *Yellow Pages* (`YP`), il **NIS** è un servizio di directory client/server progettato per centralizzare e distribuire file di configurazione di sistema all'interno di un **dominio amministrativo Unix/Linux**.

* **Obiettivo:** evitare la duplicazione manuale delle informazioni di amministrazione su ogni singolo host della rete locale.
* **Mappe NIS (*Maps*):** i file di sistema vengono convertiti in database indicizzati (DBM) distribuiti dal server ai client, tra cui:
  * `passwd` e `group` (utenti, UID/GID, home directory e password centralizzate);
  * `hosts` (risoluzione locale dei nomi host);
  * `services`, `protocols`, `netgroup` e mappe per l'*automounter*.
* **Architettura:**
  * **NIS Master Server:** detiene le copie originali autorevoli delle mappe e le propaga ai server secondari.
  * **NIS Slave Server:** mantengono repliche sincronizzate per bilanciamento del carico e tolleranza ai guasti (*fault tolerance*).
  * **NIS Client:** interrogano i server tramite chiamate **RPC** (*Remote Procedure Call*) per autenticare gli utenti e risolvere le configurazioni.

---

## 2. [[NFS]] (*Network File System*) e [[Automount]]

### [[NFS]] (*Network File System*)
Protocollo applicativo (sviluppato da Sun Microsystems) che consente a un host client di **montare e accedere a file system remoti** esportati da un server attraverso la rete come se fossero dischi locali.

* **Architettura e Protocolli di Supporto:**
  * Si basa su **[[RPC]]** (*Remote Procedure Call*) e sulla rappresentazione indipendente dall'architettura **XDR** (*External Data Representation*), permettendo la condivisione tra sistemi con architetture hardware diverse (*big-endian* vs *little-endian*).
  * Opera tipicamente su **UDP** o **TCP** (porta **2049** in NFSv3/NFSv4, con il supporto del demone `rpcbind`/`portmapper` sulla porta 111 nelle versioni precedenti).
* **Funzionamento (Server vs Client):**
  * **Lato Server:** l'amministratore dichiara nel file `/etc/exports` le directory condivise e i permessi associati (`ro`, `rw`, `root_squash`, host autorizzati).
  * **Lato Client:** tramite il comando `mount` (o la configurazione in `/etc/fstab`), il file system remoto viene agganciato a un punto di mount locale.
* **Stateless vs Stateful:**
  * **NFSv2 / NFSv3:** protocolli *stateless* (il server non mantiene lo stato dei file aperti tra una richiesta e l'altra; ogni operazione contiene tutte le informazioni necessarie, facilitando il ripristino dopo un crash).
  * **NFSv4:** introduce la gestione *stateful*, sicurezza integrata (Kerberos) e l'uso esclusivo della porta TCP 2049 senza dipendenza da `portmapper`.

### [[Automount]] (`autofs`)
Servizio demone lato client che gestisce il **montaggio dinamico (*on-demand*)** dei file system remoti (tipicamente NFS):
* **Montaggio automatico:** il file system remoto viene montato automaticamente solo nel momento in cui un utente o un processo accede effettivamente al percorso configurato (es. `/home/utente`).
* **Smontaggio automatico (*unmount*):** dopo un intervallo configurabile di inattività (*timeout*), il demone smonta la risorsa liberando risorse di rete e prevenendo blocchi dei client in caso di spegnimento del server NFS.
* **Integrazione con NIS:** le mappe di automount (`auto.master`, `auto.home`) possono essere distribuite centralmente tramite NIS.

---

## 3. [[SNMP]] (*Simple Network Management Protocol*)

Protocollo standard della suite TCP/IP (livello Applicazione) dedicato al **monitoraggio, alla gestione e alla diagnostica** degli apparati attivi di rete (router, switch, server, stampanti, UPS).

### Componenti dell'Architettura SNMP
1. **NMS (*Network Management Station* / Manager):** stazione centrale di controllo che interroga i dispositivi, raccoglie statistiche, visualizza lo stato della rete e invia comandi di configurazione.
2. **SNMP Agent:** modulo software eseguito sui singoli dispositivi gestiti (*managed devices*) che raccoglie i parametri locali e risponde alle richieste del Manager.
3. **[[MIB]] (*Management Information Base*):** database gerarchico ad albero strutturato secondo lo standard SMI (*Structure of Management Information*), in cui ogni risorsa o contatore è identificato univocamente da un **OID** (*Object Identifier*, es. `1.3.6.1.2.1...`).

### Operazioni Principali e Porte
* **Trasporto:** utilizza **[[UDP]]** per minimizzare il sovraccarico di rete:
  * **Porta UDP 161:** usata dall'Agent per ricevere le richieste del Manager (`GetRequest`, `GetNextRequest`, `GetBulkRequest`, `SetRequest`) e inviare `GetResponse`.
  * **Porta UDP 162:** usata dal Manager per ricevere segnalazioni asincrone spontanee inviate dagli Agent in caso di eventi critici (**`SNMP Trap`** o `InformRequest`).

### Evoluzione delle Versioni SNMP
* **SNMPv1:** prima versione; autenticazione debole basata su una semplice stringa in chiaro (*Community String*, es. `public` per sola lettura, `private` per lettura/scrittura).
* **SNMPv2c:** introduce miglioramenti prestazionali (`GetBulkRequest` per leggere intere tabelle in un solo pacchetto), mantenendo però l'autenticazione tramite *Community String*.
* **SNMPv3:** introduce modelli di sicurezza completi (**USM** – *User-based Security Model* e **VACM** – *View-based Access Control Model*), garantendo **autenticazione**, **integrità** dei messaggi e **cifratura** del traffico di gestione.