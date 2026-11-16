---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
argomento: Modello di Riferimento ISO/OSI e confronto con TCP/IP
tags:
  - reti
  - iso-osi
  - tcp-ip
  - protocolli
  - networking
---
# Modello di Riferimento ISO/OSI

## 1. Cos'è il Modello ISO/OSI
Il **Modello ISO/OSI** (*Open Systems Interconnection*) è un **modello concettuale** che definisce come le reti inviano i dati dal mittente al destinatario.
* **Scopo:** descrivere ogni componente della comunicazione dati per definire regole e standard su applicazioni e infrastruttura, permettendo l'interconnessione di sistemi informatici e il confronto tra protocolli diversi (proprietari e non).
* **Natura teorica:** rimane principalmente un impianto concettuale con pochissime implementazioni dirette di servizi (es. **ISO/IEC 10021 / X.400** per la posta elettronica, **ISO/IEC 9594 / X.500** per i servizi di Directory).

### Definizioni Base
* **Sistema:** insieme di risorse hardware, risorse software, periferiche e programmi.
* **Applicazione:** programma che elabora i dati ed eroga servizi.
* **Topologia logica di rete:** i **nodi** della rete sono connessi fisicamente/logicamente mediante **communication path**; i percorsi end-to-end tra due nodi sono detti **information path**.

---

## 2. Principi dell'Architettura a Livelli

La suddivisione adotta l'approccio *divide et impera*: scomporre un problema complesso in **7 sottoproblemi (livelli)** disposti concettualmente dal basso verso l'alto.

* **Gerarchia e interfacce:** tranne i livelli 1 e 7, ogni livello $N$ è collegato ai livelli adiacenti ($N-1$ e $N+1$). Ciascun livello sfrutta i servizi del livello sottostante ed eroga servizi a quello superiore tramite un'interfaccia chiamata **[[SAP]]** (*Service Access Point*).
* **Entità e Protocolli:**
  * Ogni livello è costituito da una o più **entità**.
  * Entità dello stesso livello su sistemi diversi sono dette **peer entities** e comunicano tra loro tramite un insieme di **protocolli** specifici di quel livello.

### Meccanismo di Incapsulamento (PDU, SDU, PCI)
Durante la trasmissione dal mittente al destinatario:
1. I dati **N-PDU** (*Protocol Data Unit*) generati da un protocollo di livello $N$ attraversano l'interfaccia **SAP** verso il livello $N-1$, diventando **$(N-1)$-SDU** (*Service Data Unit*).
2. Il livello $N-1$ aggiunge alle SDU le proprie informazioni di controllo **$(N-1)$-PCI** (*Protocol Control Information* — header e/o trailer), formando una nuova **$(N-1)$-PDU**.
3. Il processo si ripete fino al livello 1, dove i bit vengono trasmessi sul mezzo fisico; al destinatario avviene il processo inverso (decapsulamento).

---

## 3. I 7 Livelli del Modello ISO/OSI

I livelli si dividono in **Media Layers** (livelli 1–3, legati all'infrastruttura di trasporto) e **Host Layers** (livelli 4–7, gestiti dagli host terminali):

| # | Livello | Categoria | Funzioni principali | Unità Dati (PDU) | Apparati / Esempi di Protocolli |
| :-: | :--- | :--- | :--- | :--- | :--- |
| **7** | **Applicazione** (*Application*) | Host Layer | Finestra per utenti e processi applicativi per accedere ai servizi di rete (file transfer, email, web, directory). | Data | [[HTTP]], [[HTTPS]], [[DNS]], [[FTP]], [[SMTP]], [[POP]], [[DHCP]], [[SSH]], SNMP, X.400 |
| **6** | **Presentazione** (*Presentation*) | Host Layer | "Traduttore" della rete: sintassi, conversione codici caratteri (ASCII, EBCDIC), compressione e cifratura (*encryption/decryption*). | Data | JPEG, GIF, TIFF, PICT, ASCII, EBCDIC |
| **5** | **Sessione** (*Session*) | Host Layer | Apertura, gestione, sincronizzazione e chiusura delle sessioni tra processi su host diversi (porte logiche). | Data | RPC, SQL, NFS, NetBIOS, Socket |
| **4** | **Trasporto** (*Transport*) | Host Layer | Trasferimento *end-to-end* affidabile (o non), segmentazione, controllo di flusso e multiplexing delle sessioni. | **Segments** | [[TCP]], [[UDP]], SPX |
| **3** | **Rete** (*Network*) | Media Layer | Instradamento (*routing*) nella sottorete, determinazione del percorso e indirizzamento logico. | **Packets** | **Router**, [[IP]] (IPv4/IPv6), IPX, [[ICMP]], [[ARP]], [[RARP]] |
| **2** | **Collegamento Dati** (*Data Link*) | Media Layer | Trasferimento affidabile di frame tra nodi adiacenti, indirizzamento fisico (MAC), controllo errori e accesso al mezzo (MAC/LLC). | **Frames** | **Switch**, **Bridge**, WAP, IEEE 802.3 ([[Ethernet]]), FDDI, PPP, SLIP |
| **1** | **Fisico** (*Physical*) | Media Layer | Trasmissione di bit grezzi sul mezzo trasmissivo; definisce cavi, connettori, tensioni elettriche e codifica del segnale. | **Bits** | **Hub**, cavi Ethernet (10Base2, 10Base5, 10BaseT), fibra ottica, wireless |

> [!note] Gateway
> Nelle architetture di rete, un **Gateway** può operare su tutti i livelli del modello per interconnettere reti con protocolli diversi.

---

## 4. Sistemi Terminali e Sistemi Intermedi (*Intermediate Systems*)
* **End Systems (Host A e Host B):** implementano l'intera pila protocollare completa (tutti i **7 livelli**).
* **Transit / Intermediate Systems (es. [[Router]]):** dispositivi intermedi che collegano mezzi fisici diversi e implementano solo i **primi 3 livelli** (*Physical*, *Data Link*, *Network*) per instradare i pacchetti verso la destinazione.

---

## 5. Confronto: ISO/OSI vs Modello [[TCP-IP]] (DoD)

Mentre il modello OSI ha 7 livelli, lo stack **TCP/IP** (modello pratico su cui si basa Internet) ne prevede **4** (o 5 nelle varianti didattiche):

| Livelli ISO/OSI                                 | Livelli TCP/IP (DoD)                     | Protocolli e Identificativi Chiave          |
| :---------------------------------------------- | :--------------------------------------- | :------------------------------------------ |
| 7. Application<br>6. Presentation<br>5. Session | **Application** (*Process*)              | HTTP, HTTPS, DNS, FTP, SMTP, POP, DHCP, SSH |
| 4. Transport                                    | **Transport** (*Host-to-Host*)           | TCP, UDP (Porte)                            |
| 3. Network                                      | **Internet** (*Network*)                 | IP (Indirizzi IPv4 / IPv6), ICMP, ARP       |
| 2. Data Link<br>1. Physical                     | **Network Access** (*Network Interface*) | Indirizzi MAC, Ethernet, Wi-Fi, Fibra, Cavi |