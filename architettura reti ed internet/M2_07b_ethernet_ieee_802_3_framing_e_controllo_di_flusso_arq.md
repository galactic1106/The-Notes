---
corso: Architettura Reti ed Internet
modulo: Modulo 2 - Prof. Damiano Perri
università: Università degli Studi di Perugia (UNIPG)
fonte: parte_7.pdf
argomento: Architettura IEEE 802, Ethernet CSMA/CD, struttura del frame 802.3, framing e protocolli ARQ
tags:
  - reti
  - data-link
  - ethernet
  - ieee-802-3
  - csma-cd
  - arq
  - framing
---

# 🔗 Ethernet (IEEE 802.3), Framing e Controllo di Flusso ARQ

## 1. Architettura IEEE 802: Sottolivelli LLC e MAC
Lo standard **IEEE 802** suddivide il Livello 2 ISO/OSI (*Data Link Layer*) in due sottolivelli funzionali:
1. **[[LLC]] (*Logical Link Control* – IEEE 802.2):** sottolivello superiore indipendente dal mezzo fisico; fornisce l'interfaccia verso il livello Network (multiplexing dei protocolli tramite campi DSAP/SSAP) ed eventuale controllo di flusso e di errore.
2. **[[MAC]] (*Media Access Control*):** sottolivello inferiore dipendente dal mezzo fisico; gestisce l'indirizzamento fisico (**MAC Address** a 48 bit / 6 byte), l'incapsulamento delle trame (*framing*) e l'accesso al canale condiviso.

---

## 2. [[Ethernet]] (IEEE 802.3) e Protocollo [[CSMA-CD]]

### Accesso al Mezzo: CSMA/CD (*Carrier Sense Multiple Access with Collision Detection*)
Utilizzato nelle reti Ethernet classiche su mezzo condiviso (bus coassiale o hub *half-duplex*):
1. **Carrier Sense (Ascolto della portante):** prima di trasmettere, la scheda di rete verifica se il canale è libero (*idle*). Se è occupato (*busy*), attende.
2. **Multiple Access (Accesso multiplo):** più stazioni condividono lo stesso mezzo; se il canale è libero, la stazione inizia la trasmissione.
3. **Collision Detection (Rilevamento delle collisioni):** durante l'invio, la stazione continua ad ascoltare il mezzo. Se due stazioni trasmettono contemporaneamente si verifica una **collisione**:
   * Le stazioni interrompono immediatamente i dati e inviano un segnale di disturbo (**Jam Signal**) per avvisare tutti i nodi.
   * Ciascuna stazione attende un tempo casuale calcolato tramite l'algoritmo di **Binary Exponential Backoff** (multiplo dello *slot time*) prima di ritentare.

> [!note] Ethernet moderna Full-Duplex
> Nelle moderne reti cablate basate su **Switch** e collegamenti punto-punto *full-duplex*, ogni porta costituisce un dominio di collisione isolato: le collisioni sono eliminate alla radice e il CSMA/CD non viene più attivato.

### Struttura della Trama Ethernet (IEEE 802.3)
```text
+----------+-----+-----+-----+---------------+----------------------+-------+
| Preamble | SFD | DA  | SA  | Length / Type |      Data / LLC      |  FCS  |
|  7 byte  | 1 B | 6 B | 6 B |    2 byte     |    46 - 1500 byte    | 4 B   |
+----------+-----+-----+-----+---------------+----------------------+-------+
```
* **Preamble (7 byte) + SFD (*Start Frame Delimiter*, 1 byte `10101011`):** sincronizzano il clock del ricevitore e segnalano l'inizio effettivo del frame.
* **DA (*Destination Address*, 6 byte) e SA (*Source Address*, 6 byte):** indirizzi MAC univoci (espressi in esadecimale, es. `00:1A:2B:3C:4D:5E`); i primi 3 byte identificano il produttore (**OUI**), gli ultimi 3 la scheda.
* **Length / Type (2 byte):** se il valore è $\le 1500$ (`0x05DC`) indica la lunghezza del campo dati in byte (IEEE 802.3); se $\ge 1536$ (`0x0600`) indica il protocollo incapsulato (es. `0x0800` per IPv4, `0x0806` per ARP — formato Ethernet II).
* **Data + Pad (46 – 1500 byte):** payload trasportato; se i dati sono inferiori a **46 byte**, viene aggiunto un riempimento (*padding*) per garantire una dimensione minima della trama di **64 byte** (necessaria al rilevamento delle collisioni CSMA/CD).
* **FCS (*Frame Check Sequence*, 4 byte):** codice **CRC-32** calcolato sull'intera trama per rilevare errori di trasmissione.

---

## 3. Tecniche di Framing (Delimitazione delle Trame)
Per consentire al ricevitore di individuare con certezza l'inizio e la fine di ciascun frame all'interno del flusso continuo di bit si impiegano tre tecniche principali:
1. **Byte Count (Conteggio dei byte):** un campo nell'header specifica il numero totale di byte del frame (vulnerabile se un errore altera proprio il contatore, causando perdita di sincronismo).
2. **Character / Byte Stuffing:** delimita la trama con sequenze di caratteri speciali (es. `DLE STX` e `DLE ETX`); se il carattere di escape `DLE` appare nei dati viene raddoppiato (`DLE DLE`).
3. **Bit Stuffing:** delimita inizio e fine trama con una sequenza fissa di bit detta **Flag** (`01111110`); ogni volta che nei dati compaiono **cinque bit `1` consecutivi**, il trasmettitore inserisce automaticamente un bit `0` (che il ricevitore rimuove).

---

## 4. Controllo di Flusso e di Errore: Stop-and-Wait e [[ARQ]]

### Stop-and-Wait
* Il trasmettitore invia **un singolo frame** e si ferma in attesa del riscontro positivo (**`ACK`**) dal destinatario prima di inviare il frame successivo.
* Se il frame o l'`ACK` vanno perduti, alla scadenza del **Time-Out** il mittente ritrasmette il frame (la numerazione alternata $0 / 1$ evita duplicati in ricezione).
* **Limite:** bassa efficienza su canali con elevato prodotto banda-ritardo ($T_{\text{prop}} \gg T_{\text{trama}}$).

### Protocolli ARQ (*Automatic Repeat Request*) a Finestra Scorrevole (*Sliding Window*)
Permettono al trasmettitore di inviare più frame consecutivi senza attendere immediatamente l'`ACK` di ciascuno, mantenendo attiva una **finestra di trasmissione** di ampiezza $W$:

| Strategia ARQ | Gestione dei frame fuori sequenza | Comportamento in caso di errore |
| :--- | :--- | :--- |
| **Go-Back-N** | Il ricevitore **scarta** tutti i frame ricevuti dopo un frame perso o corrotto (finestra ricezione $W_R = 1$). | Alla scadenza del timeout (o ricezione di `NAK`), il mittente **ritrasmette l'intero blocco** a partire dal frame errato. |
| **Selective Repeat** (*Ripetizione Selettiva*) | Il ricevitore **accetta e memorizza nel buffer** i frame corretti successivi all'errore ($W_R = W_T$). | Il mittente ritrasmette **esclusivamente il singolo frame** mancante o danneggiato, massimizzando l'efficienza. |