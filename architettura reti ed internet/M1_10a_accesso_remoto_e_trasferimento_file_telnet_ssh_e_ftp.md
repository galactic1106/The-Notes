---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_l.pdf
argomento: Protocolli di accesso remoto (Telnet, SSH) e trasferimento file (FTP)
tags:
  - reti
  - livello-applicazione
  - telnet
  - ssh
  - ftp
  - sicurezza
---

# 🔐 Accesso Remoto e Trasferimento File: Telnet, SSH e FTP

## 1. Accesso Remoto: [[Telnet]] vs [[SSH]]

### [[Telnet]] (*Teletype Network*)
Protocollo applicativo (RFC 854) progettato per aprire una sessione terminale interattiva su un host remoto tramite il modello **Client/Server**:
* **Trasporto e Porta:** utilizza **[[TCP]]** sulla **porta 23**.
* **NVT (*Network Virtual Terminal*):** definisce una rappresentazione standardizzata dei caratteri di controllo per consentire il dialogo tra client e server con architetture e sistemi operativi eterogenei.
* **Criticità di sicurezza:** trasmette **tutto il traffico in chiaro** (*plaintext*), incluse credenziali di accesso (`username` e `password`) e comandi eseguiti; è perciò vulnerabile ad attacchi di intercettazione (*sniffing*) e *man-in-the-middle*. Oggi è impiegato quasi esclusivamente per test diagnostici su porte TCP o su reti isolate.

---

### [[SSH]] (*Secure Shell*)
Protocollo crittografico che sostituisce Telnet (e i vecchi comandi *rlogin* / *rsh* / *rcp*) garantendo **riservatezza**, **integrità** e **autenticazione forte** su canali non sicuri:
* **Trasporto e Porta:** utilizza **[[TCP]]** sulla **porta 22**.
* **Architettura a 3 sottoprotocolli:**
  1. **SSH Transport Layer Protocol (`SSH-TRANS`):** gestisce lo scambio iniziale delle chiavi (es. Diffie-Hellman), l'autenticazione del server tramite chiave pubblica (*host key*), la cifratura simmetrica del canale e il controllo di integrità (HMAC).
  2. **SSH User Authentication Protocol (`SSH-USERAUTH`):** verifica l'identità dell'utente tramite password (protetta dal tunnel cifrato) o **autenticazione a chiave pubblica/privata** (`~/.ssh/authorized_keys`).
  3. **SSH Connection Protocol (`SSH-CONN`):** esegue il *multiplexing* del canale cifrato in più canali logici simultanei per shell interattive, trasferimento file (**SFTP**, **SCP**) e **Port Forwarding** (*SSH Tunneling*).

> [!tip] Vantaggi principali di SSH
> Oltre all'accesso shell cifrato, SSH consente di incapsulare e proteggere traffico di altri protocolli (es. X11 forwarding o tunnel TCP locali/remoti) senza richiedere una VPN completa.

---

## 2. Trasferimento File: [[FTP]] (*File Transfer Protocol*)

Protocollo standard (RFC 959) per il trasferimento bidirezionale di file tra host su rete TCP/IP. A differenza di altri protocolli applicativi, FTP utilizza **due connessioni TCP distinte** (*out-of-band control*):

1. **Connessione di Controllo (*Control Connection* – Porta TCP 21):**
   * Resta aperta per l'intera durata della sessione utente.
   * Trasporta i comandi testuali ASCII (`USER`, `PASS`, `LIST`, `RETR`, `STOR`, `QUIT`) e i relativi codici di risposta numerici del server (es. `200 OK`, `331 Password required`, `530 Not logged in`).
2. **Connessione Dati (*Data Connection* – Porta TCP 20 in modalità attiva):**
   * Viene aperta dinamicamente ogni volta che occorre trasferire un file o l'elenco di una directory, e chiusa al termine del trasferimento.

### Modalità Attiva (*Active Mode*) vs Modalità Passiva (*Passive Mode*)

| Caratteristica | Modalità Attiva (`PORT`) | Modalità Passiva (`PASV`) |
| :--- | :--- | :--- |
| **Inizializzazione Controllo** | Client (porta effimera $N$) $\rightarrow$ Server (porta 21) | Client (porta effimera $N$) $\rightarrow$ Server (porta 21) |
| **Inizializzazione Dati** | **Server** (porta 20) $\rightarrow$ **Client** (porta $N+1$) | **Client** (porta $N+1$) $\rightarrow$ **Server** (porta effimera $P > 1023$) |
| **Interazione con Firewall** | Problematica: il firewall lato client blocca spesso le connessioni in ingresso avviate dal server | Risolve il blocco lato client, poiché entrambe le connessioni sono avviate dal client |

> [!warning] Sicurezza e alternative moderne ad FTP
> Come Telnet, l'FTP classico invia credenziali e file **in chiaro**. Per comunicazioni sicure si utilizzano:
> * **[[FTPS]] (*FTP Secure*):** aggiunge uno strato crittografico **TLS/SSL** ad FTP.
> * **[[SFTP]] (*SSH File Transfer Protocol*):** sottosistema di **SSH** (porta 22) che usa un'unica connessione cifrata per comandi e dati.