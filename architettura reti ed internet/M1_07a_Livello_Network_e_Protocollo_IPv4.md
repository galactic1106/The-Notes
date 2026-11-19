---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_g.pdf
argomento: Funzioni del Livello 3 (Network), datagramma IPv4, campi dell'header e frammentazione IP
tags:
  - reti
  - network-layer
  - ipv4
  - ip-header
  - frammentazione
---

# 🌐 Livello Network e Protocollo IPv4

## 1. Funzioni del Livello 3 (Network Layer)
Il livello Network ha il compito di trasferire pacchetti dall'host mittente all'host destinatario attraverso una rete geografica interconnessa (**internetwork**), composta da molteplici reti fisiche eterogenee unite da **[[Router]]**:

* **Indirizzamento logico globale:** assegna a ogni interfaccia di rete un identificativo univoco e gerarchico (**[[Indirizzo IP]]**), indipendente dall'indirizzo fisico (MAC) della scheda di rete sottostante.
* **Instradamento (*Routing*):** determina il percorso migliore attraverso i router intermedi per raggiungere la rete di destinazione tramite tabelle di instradamento (*routing table*).
* **Inoltro (*Forwarding*):** trasferisce localmente ciascun pacchetto in arrivo su un'interfaccia d'ingresso verso l'interfaccia d'uscita appropriata.

> [!info] Natura del servizio [[IP]] (*Internet Protocol*)
> Il protocollo IP offre un servizio **connectionless** (senza connessione) e **best-effort** (non affidabile): ogni datagramma viene instradato in modo indipendente, senza garanzia di consegna, di ordine di arrivo o di assenza di duplicati. L'affidabilità è demandata ai livelli superiori (es. **[[TCP]]**).

---

## 2. Struttura dell'Header [[IPv4]]
L'intestazione di un pacchetto IPv4 ha una dimensione minima di **$20\text{ byte}$** (estendibile fino a $60\text{ byte}$ in presenza di opzioni), organizzata in righe da $32\text{ bit}$ ($4\text{ byte}$):

| Campo | Bit | Descrizione |
| :--- | :---: | :--- |
| **Version (VER)** | $4$ | Versione del protocollo IP (valore `4` per IPv4). |
| **IHL** (*Internet Header Length*) | $4$ | Lunghezza dell'header espressa in parole da $32\text{ bit}$ (minimo $5 \rightarrow 5 \times 4 = 20\text{ byte}$, massimo $15 \rightarrow 60\text{ byte}$). |
| **ToS / DSCP + ECN** | $8$ | *Differentiated Services* ($6\text{ bit}$) per priorità **QoS** ed *Explicit Congestion Notification* ($2\text{ bit}$). |
| **Total Length** | $16$ | Lunghezza totale del datagramma (Header + Payload) in byte; dimensione massima teorica $2^{16} - 1 = 65535\text{ byte}$. |
| **Identification** | $16$ | Identificativo univoco assegnato dal mittente per raggruppare i frammenti appartenenti allo stesso datagramma originale. |
| **Flags** | $3$ | Bit 0: riservato (`0`); Bit 1: **DF** (*Don't Fragment*); Bit 2: **MF** (*More Fragments*, `1` se seguono altri frammenti, `0` sull'ultimo). |
| **Fragment Offset** | $13$ | Posizione del payload del frammento rispetto all'inizio del datagramma originale, espressa in unità di **$8\text{ byte}$** ($64\text{ bit}$). |
| **TTL** (*Time To Live*) | $8$ | Contatore di hop massimi: ogni router decrementa il TTL di $1$; se raggiunge $0$, il pacchetto viene scartato e viene inviato un errore **ICMP Time Exceeded** (evita loop infiniti). |
| **Protocol** | $8$ | Indica il protocollo di livello superiore incapsulato nel payload (es. `1` = ICMP, `6` = TCP, `17` = UDP, `89` = OSPF). |
| **Header Checksum** | $16$ | Codice di rilevamento errori calcolato **esclusivamente sull'header** IPv4; deve essere ricalcolato da ogni router poiché il TTL cambia ad ogni hop. |
| **Source IP Address** | $32$ | Indirizzo IPv4 dell'host mittente. |
| **Destination IP Address** | $32$ | Indirizzo IPv4 dell'host destinatario. |
| **Options + Padding** | $0 - 320$ | Campi opzionali (es. *Record Route*, *Timestamp*) allineati a multipli di $32\text{ bit}$. |

![[header_ipv4.svg|700]]

---

## 3. Frammentazione e Riassemblaggio IPv4
Ogni tecnologia di livello Data Link impone una dimensione massima per il campo dati del proprio frame, detta **[[MTU]]** (*Maximum Transmission Unit*, ad esempio $1500\text{ byte}$ per Ethernet II):

* Se un router deve inoltrare un datagramma IPv4 su un collegamento la cui **MTU** è inferiore alla *Total Length* del pacchetto (e il bit **DF = 0**), il router suddivide il payload in **frammenti** più piccoli, ciascuno dotato di un proprio header IPv4.
* **Regole di frammentazione:**
  1. Tutti i frammenti mantengono lo stesso campo **Identification** del datagramma originale.
  2. La dimensione del payload di ogni frammento (tranne l'ultimo) deve essere un **multiplo di $8\text{ byte}$**.
  3. Il flag **MF** è posto a `1` su tutti i frammenti tranne l'ultimo, dove **MF = 0**.
  4. Il campo **Fragment Offset** vale $\frac{\text{Byte iniziali del payload}}{8}$.
* **Riassemblaggio:** avviene **esclusivamente presso l'host destinatario finale** (mai nei router intermedi, per non rallentare l'inoltro). Se anche un solo frammento va perduto, l'intero datagramma originale viene scartato.