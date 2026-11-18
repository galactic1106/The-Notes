---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
docente: Prof. Damiano Perri
argomento: Protocollo TCP, Bit di Controllo, 3-Way Handshake e Graceful Close
tags:
  - reti
  - tcp
  - trasporto
  - handshake
  - network-security
---

# 🔄 Livello Trasporto (Livello 4): Protocollo TCP

## 1. Bit di Controllo TCP (Flags)
L'header del protocollo **TCP (*Transmission Control Protocol*)** include **6 bit di controllo (flag)** che gestiscono lo stato della connessione e la modalità di elaborazione dei segmenti:

* **`URG` (*Urgent Pointer Flag*):** indica che il campo *Urgent Pointer* è significativo e che il segmento contiene dati urgenti da elaborare con priorità.
* **`ACK` (*Acknowledgment*):** indica che il campo *Acknowledgment Number* è valido e riscontra la ricezione dei dati.
* **`PSH` (*Push*):** richiede al livello di trasporto di consegnare immediatamente i dati all'applicazione ricevente senza attendere il riempimento del buffer (*override TCP buffering*).
* **`RST` (*Reset*):** interrompe immediatamente e in modo anomalo una connessione esistente (o rifiuta un tentativo di connessione).
* **`SYN` (*Synchronize*):** sincronizza i numeri di sequenza per instaurare una nuova connessione.
* **`FIN` (*Finish*):** indica che il mittente ha terminato l'invio di dati e richiede la chiusura della connessione.

---

## 2. Apertura della Connessione: 3-Way Handshake
Per instaurare una connessione affidabile *full-duplex*, TCP utilizza una procedura a tre vie (**Three-Way Handshake**). L'uso di numeri di sequenza iniziali casuali (**ISN**, *Initial Sequence Number*) protegge la nuova sessione da eventuali segmenti ritardati appartenenti a connessioni precedenti.

1. **Passo 1 (`SYN`):**
   * **Host A $\rightarrow$ Host B:** invia un segmento con flag `SYN` attivo e il proprio numero di sequenza iniziale $\text{Seq\_no} = x$.
2. **Passo 2 (`SYN-ACK`):**
   * **Host B $\rightarrow$ Host A:** riceve il `SYN`, alloca le risorse e risponde con i flag `SYN` e `ACK` attivi, scegliendo il proprio ISN ($\text{Seq\_no} = y$) e confermando la ricezione con $\text{Ack\_no} = x + 1$.
3. **Passo 3 (`ACK`):**
   * **Host A $\rightarrow$ Host B:** riceve il `SYN-ACK` e invia un segmento di conferma con flag `ACK` attivo, $\text{Seq\_no} = x + 1$ e $\text{Ack\_no} = y + 1$.

Al termine del terzo passo, la connessione socket TCP entra nello stato **`ESTABLISHED`** e può iniziare lo scambio bidirezionale dei dati.

![[TCP_Three_Way_Handshake.svg]]
---

## 3. Chiusura della Connessione: Graceful Close (4-Way Handshake)
Poiché una connessione TCP è *full-duplex* (i due flussi di dati sono indipendenti), ciascuna direzione deve essere chiusa separatamente tramite una procedura a quattro fasi (**Graceful Close**):

1. **Chiusura del primo flusso (`FIN`):**
   * **Host A $\rightarrow$ Host B:** invia un segmento con flag `FIN` attivo (es. $\text{Seq} = 5086$) per dichiarare di non avere più dati da trasmettere.
2. **Riscontro del primo `FIN` (`ACK`):**
   * **Host B $\rightarrow$ Host A:** conferma la ricezione del `FIN` inviando $\text{Ack} = 5087$. Da questo momento il canale $A \rightarrow B$ è chiuso, ma **Host B può continuare a inviare dati** verso Host A (es. $\text{Data, Seq} = 303, \text{Ack} = 5087$, riscontrato da A con $\text{Ack} = 453$).
3. **Chiusura del secondo flusso (`FIN`):**
   * **Host B $\rightarrow$ Host A:** terminato l'invio dei propri dati residui, invia a sua volta un segmento con flag `FIN` attivo (es. $\text{Seq} = 453, \text{Ack} = 5087$).
4. **Riscontro finale (`ACK`) e `TIME-WAIT`:**
   * **Host A $\rightarrow$ Host B:** risponde con l'ultimo riscontro ($\text{Ack} = 454$).
   * **Stato `TIME-WAIT` ($2 \times \text{MSL}$):** Host A non chiude immediatamente il socket, ma attende un intervallo pari a **$2 \times \text{MSL}$** (*Maximum Segment Lifetime*, tempo massimo di vita di un pacchetto in rete). Questo garantisce che, se l'ultimo `ACK` andasse perso e Host B ritrasmettesse il `FIN`, Host A possa riscontrarlo nuovamente evitando che pacchetti residui interferiscano con future connessioni sulla stessa coppia di porte.

![[TCP_Four_Way_Handshake_Graceful_Close.svg]]