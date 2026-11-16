# 04c — Controllo di Accesso al Mezzo (MAC) e Protocolli Data Link

## 1. Il Problema dell'Accesso Multiplo
Su canali **broadcast / condivisi** (bus, cavi coassiali, wireless, reti satellitari), più stazioni trasmettono sullo stesso mezzo fisico: se due stazioni trasmettono contemporaneamente, i segnali si sovrappongono generando una **collisione** che rende il frame illeggibile.

---

## 2. Protocolli ad Accesso Casuale (Contention-based)

### ALOHA Puro e Slotted ALOHA
* **ALOHA Puro:** ogni stazione trasmette appena ha un frame pronto. Se non riceve ACK (collisione), attende un tempo casuale (backoff) e ritrasmette.
  * Tempo di vulnerabilità: $2 T_{tx}$.
  * Throughput massimo: $S_{max} = \frac{1}{2e} \approx 18,4\%$ (raggiunto per carico offerto $G = 0,5$).
* **Slotted ALOHA:** il tempo è diviso in slot discreti di durata $T_{tx}$; le stazioni possono iniziare a trasmettere solo all'inizio di uno slot.
  * Tempo di vulnerabilità dimezzato: $T_{tx}$.
  * Throughput massimo raddoppiato: $S_{max} = \frac{1}{e} \approx 36,8\%$ (per $G = 1$).

### CSMA (Carrier Sense Multiple Access) — "Ascolta prima di parlare"
Prima di trasmettere, la stazione rileva se il canale è libero o occupato:
* **1-persistent CSMA:** se il canale è libero trasmette subito ($p=1$); se occupato attende continuamente e trasmette appena si libera (rischio collisione se due stazioni attendevano).
* **Non-persistent CSMA:** se il canale è occupato, attende un intervallo casuale prima di riascoltare (meno collisioni, maggiore ritardo).
* **p-persistent CSMA (canali slottati):** se il canale è libero, trasmette nello slot corrente con probabilità $p$ o rimanda allo slot successivo con probabilità $1-p$.

### CSMA/CD (Collision Detection — Ethernet cablata IEEE 802.3)
* Mentre trasmette, la stazione continua ad ascoltare il mezzo confrontando il segnale inviato con quello sul cavo.
* Se rileva una **collisione**, interrompe subito la trasmissione, invia un segnale di **Jam** (per assicurarsi che tutti i nodi rilevino l'evento) ed esegue l'algoritmo di **Binary Exponential Backoff (BEB)**: dopo l'$i$-esima collisione ($i \le 10$), attende un numero casuale di slot scelto in $[0, 2^i - 1]$.
* **Vincolo temporale fondamentale:** per garantire che il mittente rilevi la collisione prima di aver terminato l'invio del frame, il tempo di trasmissione del frame minimo deve essere almeno pari al Round-Trip Propagation Time ($RTT = 2 T_{prop}$):
  $$T_{tx,\min} \ge 2 T_{prop} \implies L_{\min} \ge 2 \cdot T_{prop} \cdot R$$
  *(In Ethernet classica a 10 Mbps, questo vincolo fissa la dimensione minima del frame a **64 byte** = 512 bit).*

### CSMA/CA (Collision Avoidance — Wi-Fi IEEE 802.11)
* Sul mezzo radio **non è possibile** rilevare collisioni durante la trasmissione (il segnale locale copre quello remoto) e sussiste il problema del **terminale nascosto** (hidden node).
* Usa attese casuali (Contention Window), intervalli inter-frame (`DIFS`, `SIFS`), ACK espliciti e meccanismo opzionale di prenotazione del canale tramite frame brevi **`RTS` (Request To Send)** e **`CTS` (Clear To Send)** (Virtual Carrier Sensing via NAV).

---

## 3. Protocolli senza Contesa e a Suddivisione di Canale
* **Channel Partitioning:** TDMA (divisione di tempo), FDMA (divisione di frequenza), CDMA (divisione di codice). Efficienti a carico alto e costante, inefficienti a carico basso/impulsivo.
* **Controlled Access (Turn-taking):**
  * **Polling:** un nodo primario (master) interroga ciclicamente i nodi secondari (slave).
  * **Token Passing (es. Token Ring):** un frame speciale di controllo (*token*) circola lungo un anello logico/fisico; solo chi possiede il token può trasmettere.

---

## 4. Protocolli Data Link Classici e Punto-Punto

### BSC (Binary Synchronous Communications)[cite: 4]
* Protocollo orientato ai **caratteri** (sviluppato da IBM), half-duplex, basato su codici ASCII/EBCDIC e caratteri di controllo (`SYN` per sincronizzazione, `SOH`, `STX`, `ETX`, `ACK0/ACK1`, `NAK`)[cite: 4]. Usa **byte stuffing** (`DLE`) per la trasparenza dei dati e controllo di flusso Stop-and-Wait.

### HDLC (High-Level Data Link Control)[cite: 4]
* Protocollo standard ISO orientato ai **bit**, full-duplex, con **bit stuffing** e finestra scorrevole (Go-Back-N / Selective Repeat)[cite: 4].
* **Struttura del frame HDLC:**
  `| Flag (8 bit: 01111110) | Address (8 bit) | Control (8/16 bit) | Information (variabile) | FCS/CRC (16/32 bit) | Flag (8 bit) |`
* **Tre tipi di frame** (distinti dai primi bit del campo *Control*):
  1. **I-frame (Information):** trasportano dati utente e ACK in piggybacking (`N(S)` e `N(R)`).
  2. **S-frame (Supervisory):** controllo di flusso ed errori (`RR`, `RNR`, `REJ`, `SREJ`).
  3. **U-frame (Unnumbered):** gestione della connessione e configurazione link (`SABM`, `UA`, `DISC`, ecc.).

### PPP (Point-to-Point Protocol — RFC 1661)
* Evoluzione diretta della struttura HDLC per collegamenti punto-punto su linee seriali, modem, DSL e tunnel (PPPoE).
* **Componenti principali:**
  1. **Framing tipo HDLC** (con campo *Protocol* da 1 o 2 byte per multiplexare più protocolli di rete: IPv4, IPv6, ecc.).
  2. **LCP (Link Control Protocol):** instaura, configura, testa e chiude il collegamento fisico, negoziando opzioni (es. MRU, autenticazione).
  3. **Autenticazione:** **PAP** (password in chiaro, 2-way handshake) e **CHAP** (challenge-response con hash MD5, 3-way handshake).
  4. **NCP (Network Control Protocols):** famiglia di protocolli per negoziare parametri del livello di rete specifico (es. **IPCP** per assegnare dinamicamente l'indirizzo IP al client).