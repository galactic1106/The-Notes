---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_i.pdf
argomento: Livello Trasporto (Livello 4 ISO/OSI), multiplexing/demultiplexing, porte, UDP e TCP (header e Three-Way Handshake)
tags:
  - reti
  - transport-layer
  - tcp
  - udp
  - socket
  - three-way-handshake
---

# 🚚 Livello Trasporto: UDP e TCP

## 1. Ruolo del Livello 4 (Transport Layer)
Mentre il livello Network (**[[IP]]**) realizza la comunicazione logica tra **host** (*host-to-host*), il livello Trasporto fornisce la comunicazione logica **end-to-end tra processi applicativi** in esecuzione su host remoti:

* **Multiplexing (al mittente):** raccoglie blocchi di dati da diverse socket applicative, aggiunge a ciascuno un header di trasporto creando un **segmento** (o datagramma UDP) e li passa al livello Network.
* **Demultiplexing (al destinatario):** esamina i campi dell'header di trasporto per consegnare ogni segmento ricevuto alla **socket** del processo applicativo corretto.
* **Indirizzamento tramite Porte (*Port Numbers*):** identificativi a **$16\text{ bit}$** (valori da $0$ a $65535$):
  * *Well-Known Ports ($0 - 1023$):* riservate ai servizi standard (es. `20/21` FTP, `22` SSH, `25` SMTP, `53` DNS, `80` HTTP, `110` POP3, `143` IMAP, `443` HTTPS).
  * *Registered Ports ($1024 - 49151$):* assegnate a specifici applicativi utente.
  * *Dynamic / Ephemeral Ports ($49152 - 65535$):* assegnate temporaneamente dal sistema operativo ai client per le connessioni in uscita.
* **[[Socket]]:** coppia `(Indirizzo IP, Numero di Porta)` che identifica univocamente un endpoint di comunicazione sulla rete.

---

## 2. [[UDP]] (*User Datagram Protocol* – RFC 768)
Protocollo di trasporto minimale, **connectionless** (senza connessione) e **non affidabile** (*best-effort*):

* **Caratteristiche:** nessuna instaurazione di connessione (*zero handshake delay*), nessun controllo di flusso, nessun controllo di congestione e nessuna ritrasmissione in caso di perdita o arrivo fuori ordine.
* **Header fisso e leggero ($8\text{ byte}$):**
  1. *Source Port* ($16\text{ bit}$)
  2. *Destination Port* ($16\text{ bit}$)
  3. *Length* ($16\text{ bit}$): lunghezza totale (header + payload) in byte.
  4. *Checksum* ($16\text{ bit}$): rilevamento errori (calcolato su pseudo-header IP + header UDP + dati).
* **Ambiti d'uso:** applicazioni sensibili al ritardo (*real-time*) che tollerano piccole perdite (streaming audio/video, VoIP, gaming online) oppure protocolli transazionali rapidi (*query/response*) come **[[DNS]]**, **[[DHCP]]** e **SNMP**.

---

## 3. [[TCP]] (*Transmission Control Protocol* – RFC 793)
Protocollo di trasporto **connection-oriented** (orientato alla connessione), **affidabile**, **full-duplex** e orientato al **flusso di byte (*byte-stream*)**:

### Struttura dell'Header TCP ($20\text{ byte}$ minimi, fino a $60\text{ byte}$)
* **Source Port ($16\text{ bit}$) e Destination Port ($16\text{ bit}$):** identificano le applicazioni mittente e destinataria.
* **Sequence Number ($32\text{ bit}$):** numero d'ordine del primo byte di dati trasportato nel segmento corrente all'interno del flusso.
* **Acknowledgment Number ($32\text{ bit}$):** numero del prossimo byte che il ricevente si attende di ricevere (**riscontro cumulativo**).
* **Header Length / Data Offset ($4\text{ bit}$):** lunghezza dell'header TCP espressa in parole da $32\text{ bit}$.
* **Flag di Controllo ($6\text{ bit}$ principali):**
  * `URG`: indica dati urgenti (*Urgent Pointer*).
  * `ACK`: conferma che il campo *Acknowledgment Number* è valido.
  * `PSH`: spinge i dati immediatamente verso l'applicazione senza attendere il riempimento del buffer.
  * `RST`: interrompe bruscamente una connessione in caso di errore o porta chiusa.
  * `SYN`: sincronizza i numeri di sequenza per aprire una connessione.
  * `FIN`: segnala che il mittente ha terminato di inviare dati (chiusura ordinata).
* **Receive Window (`rwnd`, $16\text{ bit}$):** spazio libero disponibile nel buffer di ricezione per il **controllo di flusso**.
* **Checksum ($16\text{ bit}$):** controllo integrità obbligatorio.
* **Options ($0 - 40\text{ byte}$):** es. **MSS** (*Maximum Segment Size*), *Window Scaling*, *SACK* (*Selective ACK*).

---

## 4. Gestione della Connessione TCP: *Three-Way Handshake*

### Apertura della Connessione (*3-Way Handshake*)
1. **Passo 1 (`SYN`):** il client invia un segmento con flag `SYN = 1` e sceglie un numero di sequenza iniziale casuale $\text{Seq} = x$.
2. **Passo 2 (`SYN-ACK`):** il server alloca i buffer e risponde con flag `SYN = 1`, `ACK = 1`, il proprio numero di sequenza iniziale $\text{Seq} = y$ e $\text{Ack} = x + 1$.
3. **Passo 3 (`ACK`):** il client conferma ricevendo i parametri del server con flag `ACK = 1`, $\text{Seq} = x + 1$ e $\text{Ack} = y + 1$ (può già trasportare payload applicativo).

### Chiusura della Connessione (*4-Way Handshake*)
Poiché TCP è full-duplex, ciascuna direzione va chiusa indipendentemente:
1. Il lato $A$ invia un segmento con `FIN = 1`.
2. Il lato $B$ risponde con `ACK = 1` (la direzione $A \rightarrow B$ è chiusa; $B$ può ancora inviare dati).
3. Quando anche $B$ ha finito, invia un segmento con `FIN = 1`.
4. $A$ risponde con `ACK = 1` ed entra nello stato temporaneo **`TIME_WAIT`** (pari a $2 \times \text{MSL}$, *Maximum Segment Lifetime*) per assicurarsi che l'ultimo ACK sia giunto a destinazione ed evitare che pacchetti ritardati interferiscano con nuove connessioni.