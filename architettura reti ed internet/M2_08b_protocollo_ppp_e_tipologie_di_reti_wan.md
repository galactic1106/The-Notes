---
corso: Architettura Reti ed Internet
modulo: Modulo 2 - Prof. Damiano Perri
università: Università degli Studi di Perugia (UNIPG)
fonte: parte_8.pdf
argomento: Protocollo Point-to-Point (PPP), autenticazione PAP/CHAP e tecnologie di reti WAN
tags:
  - reti
  - ppp
  - wan
  - x25
  - frame-relay
  - atm
---

# 🌐 Protocollo PPP e Tipologie di Reti WAN

## 1. [[PPP]] (*Point-to-Point Protocol*)
Il **PPP** (RFC 1661) è il protocollo standard di Livello 2 progettato per trasportare datagrammi multiprotocollo (IPv4, IPv6, IPX, ecc.) su **collegamenti seriali punto-punto** (linee telefoniche dial-up, linee dedicate, collegamenti SONET/SDH e **PPPoE** sulle reti d'accesso xDSL/FTTH).

### Componenti dell'Architettura PPP
1. **Incapsulamento derivato da HDLC:** incapsula pacchetti di diversi protocolli di livello Network all'interno di frame delimitati dal flag `01111110` (`0x7E`), utilizzando il **Byte Stuffing** su linee asincrone o il **Bit Stuffing** su linee sincrone.
2. **LCP (*Link Control Protocol*):** protocollo di servizio incaricato di negoziare le opzioni di configurazione del collegamento (es. dimensione massima della trama **MRU**, compressione, rilevamento di loop tramite *Magic Number*), verificare la qualità del link e chiudere la connessione.
3. **Protocolli di Autenticazione (opzionali):**
   * **[[PAP]] (*Password Authentication Protocol*):** autenticazione semplice a due vie in cui il client invia `username` e `password` **in chiaro**; vulnerabile all'intercettazione.
   * **[[CHAP]] (*Challenge-Handshake Authentication Protocol*):** autenticazione sicura a tre vie (*challenge-response*) basata su funzione hash (es. MD5). Il server invia una sfida casuale (*challenge*), il client calcola l'hash combinando la sfida con la password condivisa (mai trasmessa in rete) e restituisce il risultato; il server può ripetere la verifica periodicamente durante la sessione.
4. **NCP (*Network Control Protocols*):** famiglia di protocolli specifici per ciascun protocollo di livello 3 trasportato (es. **IPCP** per configurare dinamicamente l'indirizzo IP e i server DNS del client durante la connessione).

### Formato del Frame PPP
```text
+----------+----------+----------+----------+-----------------------+----------+----------+
|   Flag   | Address  | Control  | Protocol |      Information      |   FCS    |   Flag   |
|  1 byte  |  1 byte  |  1 byte  | 1-2 byte |   Fino a 1500 byte    | 2-4 byte |  1 byte  |
+----------+----------+----------+----------+-----------------------+----------+----------+
   0x7E        0xFF       0x03     es. 0x0021 (IPv4)
```
* **Address (`0xFF`) e Control (`0x03`):** valori fissi ereditati dalla modalità *Unnumbered Information* di HDLC (su un collegamento punto-punto non occorre indirizzare stazioni multiple).
* **Protocol:** identifica il protocollo incapsulato nel payload (es. `0x0021` per IPv4, `0xC021` per LCP, `0xC023` per PAP, `0xC223` per CHAP, `0x8021` per IPCP).

---

## 2. Strutture di Rete e Tipologie di Reti [[WAN]]
Le reti geografiche (**WAN – *Wide Area Network***) interconnettono LAN distribuite su vaste aree geografiche utilizzando infrastrutture fornite da operatori di telecomunicazioni (*Carrier*).

### Collegamenti Dedicati vs Reti Commutate
* **Linee Dedicate (*Leased Lines*):** collegamenti punto-punto permanenti riservati esclusivamente al cliente (es. circuiti **T1** a $1,544\text{ Mbit/s}$ o **E1** a $2,048\text{ Mbit/s}$); offrono banda garantita e latenza costante a fronte di costi elevati.
* **Reti a Commutazione di Circuito:** instaurano un percorso fisico dedicato solo per la durata della chiamata (es. rete telefonica **PSTN** e reti digitali **ISDN**).
* **Reti a Commutazione di Pacchetto / Cella:** condividono statisticamente la capacità della dorsale tra più utenti tramite circuiti virtuali (**SVC** – *Switched Virtual Circuit* o **PVC** – *Permanent Virtual Circuit*).

### Principali Tecnologie WAN Classiche
| Tecnologia | Unità dati | Caratteristiche principali |
| :--- | :--- | :--- |
| **[[X.25]]** | Pacchetto (lunghezza variabile) | Primo standard WAN a commutazione di pacchetto (CCITT); implementa un pesante controllo di flusso e di errore ad ogni singolo nodo (*hop-by-hop*) per compensare l'inaffidabilità delle vecchie linee in rame, limitando la velocità intorno a $64\text{ kbit/s}$. |
| **[[Frame Relay]]** | Frame (lunghezza variabile) | Evoluzione snella di X.25 pensata per mezzi trasmissivi più affidabili (fibra ottica); elimina il controllo di errore hop-by-hop delegandolo ai livelli superiori e usa identificatori **DLCI** (*Data Link Connection Identifier*), raggiungendo velocità fino a $2\text{–}45\text{ Mbit/s}$. |
| **[[ATM]]** (*Asynchronous Transfer Mode*) | Cella a dimensione fissa (**53 byte**: 5 header + 48 payload) | Progettato per integrare voce, video e dati (*B-ISDN*) con bassa latenza e garanzie rigorose di **QoS** (*Quality of Service*) tramite commutazione hardware ad altissima velocità. |

> [!tip] Evoluzione moderna delle WAN
> Nelle reti moderne le tecnologie X.25, Frame Relay e ATM sono state progressivamente sostituite da dorsali **IP/[[MPLS]]** (*Multiprotocol Label Switching*), **Carrier Ethernet** e collegamenti **SD-WAN** su fibra ottica.