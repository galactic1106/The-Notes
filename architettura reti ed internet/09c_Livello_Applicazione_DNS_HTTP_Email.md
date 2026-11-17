---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_i.pdf
argomento: Livello Applicazione, architetture Client-Server e P2P, DNS, HTTP/HTTPS e protocolli di posta elettronica (SMTP, POP3, IMAP)
tags:
  - reti
  - application-layer
  - dns
  - http
  - https
  - smtp
  - imap
---

# 🌍 Livello Applicazione: DNS, Web (HTTP) e Posta Elettronica

## 1. Architetture Applicative di Rete
Le applicazioni distribuite si basano su due paradigmi architetturali principali (o su modelli ibridi):
* **Architettura Client-Server:** un **Server** sempre attivo (*always-on*) con indirizzo IP fisso e noto eroga servizi a molteplici **Client** che avviano la comunicazione in modo discontinuo (es. Web, Email, DNS).
* **Architettura Peer-to-Peer ([[P2P]]):** assenza di server centrali dedicati; coppie di host intermittenti (**peer**) comunicano direttamente tra loro scambiandosi risorse e garantendo **auto-scalabilità** (es. BitTorrent).

---

## 2. [[DNS]] (*Domain Name System*)
Il **DNS** è un database distribuito e gerarchico (implementato su **UDP porta `53`**, o TCP `53` per risposte grandi e *zone transfer*) che traduce i nomi simbolici dei domini (FQDN, es. `www.unipg.it`) nei corrispondenti **[[Indirizzo IP]]** (e viceversa):

### Gerarchia dei Server DNS
1. **Root DNS Server (`.`):** 13 cluster logici distribuiti globalmente (*anycast*) che indirizzano verso i server TLD.
2. **TLD (*Top-Level Domain*) Server:** gestiscono i domini di primo livello generici (`.com`, `.org`, `.net`) e nazionali (`.it`, `.uk`, `.de`).
3. **Authoritative DNS Server:** server autorevoli che detengono i record ufficiali di un'organizzazione (es. i server DNS dell'Università di Perugia per `unipg.it`).
4. **Local DNS Server (Resolver):** server ricorsivo fornito dall'ISP o dall'organizzazione locale che interroga la gerarchia per conto del client e memorizza i risultati nella propria **DNS Cache** per un tempo pari al **TTL** (*Time To Live*).

### Risoluzione Ricorsiva vs Iterativa
* **Query Ricorsiva:** il client delega interamente al *Local DNS Server* l'onere di ottenere la risposta finale.
* **Query Iterativa:** il *Local DNS Server* interroga in sequenza Root, TLD e Authoritative Server, ricevendo da ciascuno il riferimento (*referral*) al livello successivo.

### Principali Tipi di Record DNS (`Resource Records`)
* **`A` / `AAAA`:** associa un nome host a un indirizzo **IPv4** (`A`) o **IPv6** (`AAAA`).
* **`NS` (*Name Server*):** indica il server DNS autorevole per una determinata zona.
* **`CNAME` (*Canonical Name*):** definisce un alias (nome canonico) per un altro nome di dominio.
* **`MX` (*Mail Exchange*):** identifica il server di posta elettronica incaricato di ricevere le email per quel dominio.

---

## 3. Il Web: [[HTTP]] e [[HTTPS]]
Il protocollo **HTTP** (*HyperText Transfer Protocol*) governa lo scambio di risorse ipertestuali tra browser (client) e Web server (porta `80`):

* **Protocollo Stateless:** il server HTTP non conserva memoria delle richieste precedenti dello stesso client. Per mantenere lo stato delle sessioni utente (es. autenticazione, carrello) si utilizzano i **Cookie** (memorizzati lato client) e le **Sessioni** lato server.
* **Connessioni Non Persistenti (HTTP/1.0) vs Persistenti (HTTP/1.1):**
  * In HTTP/1.0 si apre una nuova connessione TCP per ogni singolo oggetto della pagina (elevato overhead).
  * In HTTP/1.1 (*Keep-Alive*) la connessione TCP rimane aperta per trasmettere più richieste e risposte consecutive (con supporto al *pipelining*).
* **Evoluzioni moderne:** **HTTP/2** (multiplexing binario dei flussi su un'unica connessione TCP, compressione header HPACK) e **HTTP/3** (basato su protocollo **QUIC** sopra **UDP** per eliminare l'*Head-of-Line Blocking* di TCP).
* **Principali Metodi HTTP:** `GET` (richiede una risorsa), `POST` (invia dati da elaborare, es. form), `PUT` (carica/sostituisce una risorsa), `DELETE` (rimuove una risorsa), `HEAD` (richiede solo gli header senza corpo).
* **Codici di Stato principali:** `200 OK`, `301 Moved Permanently`, `400 Bad Request`, `403 Forbidden`, `404 Not Found`, `500 Internal Server Error`.
* **[[HTTPS]] (HTTP Secure – porta `443`):** incapsula HTTP all'interno di un canale cifrato **[[TLS]]** (*Transport Layer Security*), garantendo **riservatezza** (crittografia simmetrica negoziata via scambio chiavi asimmetrico), **integrità** (MAC/HMAC) e **autenticazione** del server tramite **Certificati Digitali X.509** firmati da una **CA** (*Certification Authority*).

---

## 4. Posta Elettronica: [[SMTP]], [[POP3]] e [[IMAP]]
L'architettura della posta elettronica si compone di **User Agent** (client mail), **Mail Server** (con caselle utente e code di uscita) e tre protocolli applicativi principali:

* **[[SMTP]] (*Simple Mail Transfer Protocol* – porta `25`, oppure `587` / `465` con TLS):** protocollo *push* usato per **inviare** i messaggi dal client al proprio server di posta e per **trasferirli** tra il server del mittente e quello del destinatario (trasporta solo testo ASCII a 7 bit; gli allegati binari vengono codificati tramite lo standard **MIME**).
* **[[POP3]] (*Post Office Protocol v3* – porta `110` / `995`):** protocollo *pull* semplice che permette al client di scaricare i messaggi sul dispositivo locale (spesso cancellandoli dal server).
* **[[IMAP]] (*Internet Message Access Protocol* – porta `143` / `993`):** protocollo *pull* evoluto che mantiene i messaggi **sincronizzati direttamente sul server**, consentendo la gestione di cartelle remote e l'accesso da più dispositivi contemporaneamente.