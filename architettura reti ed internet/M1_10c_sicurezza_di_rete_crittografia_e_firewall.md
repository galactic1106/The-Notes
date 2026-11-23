---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_l.pdf
argomento: Principi di sicurezza di rete, crittografia, TLS/SSL e architetture Firewall
tags:
  - reti
  - sicurezza
  - crittografia
  - firewall
  - dmz
  - tls
---

# 🛡️ Sicurezza di Rete, Crittografia e Firewall

## 1. Proprietà Fondamentali della Sicurezza (Triade CIA e oltre)
La protezione delle informazioni trasmesse su reti aperte si fonda su quattro requisiti principali:
* **Riservatezza (*Confidentiality*):** solo il mittente e il destinatario autorizzato possono comprendere il contenuto del messaggio (garantita tramite **crittografia**).
* **Integrità (*Integrity*):** garanzia che i dati non siano stati alterati, modificati o corrotti durante il transito (garantita tramite funzioni di **hash crittografico** e **MAC/HMAC**).
* **Autenticazione (*Authentication*):** verifica certa dell'identità delle parti coinvolte nella comunicazione.
* **Disponibilità (*Availability*) e Non Ripudio (*Non-repudiation*):** continuità operativa dei servizi (protezione da attacchi **DoS / DDoS**) e impossibilità per il mittente di negare l'invio di un messaggio firmato digitalmente.

---

## 2. Fondamenti di Crittografia e Canali Sicuri

### Crittografia Simmetrica (*Secret Key*)
* Mittente e destinatario condividono la **stessa chiave segreta** $K$ per cifrare e decifrare il messaggio.
* **Vantaggi:** elevatissima velocità computazionale, ideale per cifrare grandi volumi di dati (es. **AES**, ChaCha20, 3DES).
* **Limiti:** problema della distribuzione sicura della chiave segreta su canale non protetto e scalabilità ($O(n^2)$ chiavi necessarie per $n$ utenti).

### Crittografia Asimmetrica (*Public Key*)
* Ogni entità possiede una coppia di chiavi matematicamente correlate: una **chiave pubblica** ($K_{\text{pub}}$, distribuibile liberamente) e una **chiave privata** ($K_{\text{priv}}$, custodita segretamente) (es. **RSA**, **ECC**).
* **Applicazioni:**
  * **Riservatezza:** il mittente cifra con $K_{\text{pub}}$ del destinatario; solo il destinatario può decifrare con la propria $K_{\text{priv}}$.
  * **Firma Digitale (*Digital Signature*):** il mittente firma l'impronta (*hash*) del messaggio con la propria $K_{\text{priv}}$; chiunque può verificare autenticità, integrità e non ripudio usando $K_{\text{pub}}$.

### Certificati Digitali, PKI e [[TLS]]/[[SSL]]
* **Certification Authority (CA) e Certificati X.509:** un'autorità terza fidata firma digitalmente un certificato che associa in modo verificabile un'identità (es. nome a dominio) alla rispettiva chiave pubblica.
* **Crittografia Ibrida ([[TLS]] / [[HTTPS]]):** durante l'*handshake* TLS si utilizza la **crittografia asimmetrica** per autenticare il server tramite certificato X.509 e concordare una **chiave di sessione** temporanea, per poi proseguire lo scambio dati con la veloce **crittografia simmetrica**.

---

## 3. [[Firewall]] e Architetture di Sicurezza Perimetrale

Un **Firewall** è un componente hardware o software posto al confine tra due o più reti (tipicamente tra una rete interna aziendale fidata e Internet) che filtra il traffico in ingresso e in uscita in base a un insieme di **regole di sicurezza (*Access Control List – ACL*)**.

### Tipologie Principali di Firewall
1. **Packet Filtering (Stateless – Livelli 3 e 4):**
   * Analizza ogni singolo pacchetto in modo indipendente ispezionando gli header **IP** (indirizzi sorgente/destinazione, protocollo) e **TCP/UDP** (porte sorgente/destinazione, flag TCP come `SYN`/`ACK`).
   * **Pro/Contro:** molto veloce e trasparente, ma non mantiene memoria dello stato delle connessioni né ispeziona il contenuto applicativo.
2. **Stateful Inspection Firewall (Livelli 3–4 con tabella di stato):**
   * Mantiene una **tabella di stato delle connessioni attive** (*State Table*), distinguendo i pacchetti appartenenti a sessioni legittime già avviate dall'interno da pacchetti non sollecitati provenienti dall'esterno.
3. **Application-Level Gateway / Proxy Firewall (Livello 7):**
   * Opera a livello Applicazione agendo da intermediario (*proxy*) per protocolli specifici (HTTP, SMTP, DNS). Termina la connessione del client, ispeziona il payload applicativo e apre una nuova connessione verso la destinazione.
   * **Pro/Contro:** controllo granulare e mascheramento diretto della rete interna, a fronte di un maggiore impatto sulle prestazioni.
4. **Next-Generation Firewall (NGFW):**
   * Integra *stateful inspection*, **Deep Packet Inspection (DPI)**, riconoscimento delle applicazioni, sistemi **IPS** (*Intrusion Prevention System*) e ispezione del traffico cifrato.

### Architetture Perimetrali e [[DMZ]]
* **Bastion Host:** sistema fortemente rinforzato (*hardened*), esposto direttamente alla rete esterna per ospitare proxy o servizi perimetrali, riducendo al minimo i servizi attivi e la superficie d'attacco.
* **Screened Subnet / [[DMZ]] (*Demilitarized Zone*):** architettura a due firewall (esterno e interno) che isola i server pubblici (Web, Mail, DNS esterni) in una sottorete intermedia:
  * Il **firewall esterno** consente l'accesso da Internet ai soli servizi pubblici della DMZ.
  * Il **firewall interno** protegge la **Intranet** aziendale consentendo il traffico dalla LAN verso DMZ/Internet, ma bloccando qualsiasi connessione diretta avviata dalla DMZ verso la rete interna.