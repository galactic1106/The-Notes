---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_i.pdf
argomento: Affidabilità in TCP, ritrasmissione, controllo di flusso (rwnd) e controllo di congestione (cwnd, Slow Start, Congestion Avoidance, Fast Retransmit)
tags:
  - reti
  - tcp
  - flow-control
  - congestion-control
  - sliding-window
---

# ⚖️ TCP: Affidabilità, Controllo di Flusso e Congestione

## 1. Trasferimento Affidabile e Ritrasmissione
Per garantire la consegna ordinata e priva di perdite sopra il livello IP (*best-effort*), TCP utilizza:
* **Riscontri Cumulativi (`ACK`):** il destinatario conferma tutti i byte ricevuti contiguamente fino a un certo indice.
* **Timer di Ritrasmissione (**RTO** – *Retransmission Timeout*):** calcolato dinamicamente stimando il tempo medio di andata e ritorno (**EstimatedRTT**) e la sua variabilità (**DevRTT**):
  $$\text{RTO} = \text{EstimatedRTT} + 4 \times \text{DevRTT}$$
* **Fast Retransmit (Ritrasmissione Rapida):** se il mittente riceve **3 ACK duplicati** consecutivi per lo stesso numero di sequenza, deduce che quel segmento è andato perduto e lo ritrasmette **immediatamente**, senza attendere la scadenza del timer RTO.

---

## 2. Controllo di Flusso (*Flow Control*)
Il controllo di flusso serve a **evitare che un mittente veloce saturi il buffer di ricezione di un destinatario più lento**:

* Il ricevente mantiene un buffer di capacità totale $\text{RcvBuffer}$ e comunica continuamente al mittente lo spazio attualmente libero nel campo **Receive Window (`rwnd`)** dell'header TCP:
  $$\text{rwnd} = \text{RcvBuffer} - (\text{LastByteRcvd} - \text{LastByteRead})$$
* Il mittente limita la quantità di dati inviati e non ancora riscontrati (*in flight*) in modo che sia sempre:
  $$\text{LastByteSent} - \text{LastByteAcked} \le \text{rwnd}$$
* *(Se `rwnd = 0`, il mittente sospende l'invio ma trasmette periodicamente piccoli segmenti di sonda da $1\text{ byte}$ per conoscere quando il buffer torna libero ed evitare deadlock).*

---

## 3. Controllo di Congestione (*Congestion Control*)
A differenza del controllo di flusso (che tutela il singolo ricevente), il controllo di congestione **tutela l'intera rete intermedia (router e code)** dal sovraccarico:

* Il mittente mantiene una variabile interna detta **Congestion Window (`cwnd`)**, espressa in byte o multipli di **MSS** (*Maximum Segment Size*).
* La finestra effettiva di trasmissione del mittente è limitata dal minimo tra la capacità del ricevente e quella stimata della rete:
  $$\text{Finestra Effettiva} = \min(\text{cwnd}, \text{rwnd})$$

### Fasi dell'Algoritmo di Controllo di Congestione TCP (AIMD)
TCP sonda la capacità disponibile della rete tramite l'approccio **AIMD** (*Additive Increase, Multiplicative Decrease*) articolato in tre stati:

1. **Slow Start (Partenza Lenta):**
   * All'inizio della connessione, `cwnd` parte da un valore piccolo (tipicamente $1$ o $10\text{ MSS}$).
   * Per ogni ACK ricevuto, `cwnd` viene incrementata di $1\text{ MSS}$, provocando un **raddoppio esponenziale** di `cwnd` ad ogni RTT fino al raggiungimento della soglia **`ssthresh`** (*Slow Start Threshold*) o di una perdita.
2. **Congestion Avoidance (Prevenzione della Congestione):**
   * Superata la soglia `ssthresh` ($\text{cwnd} \ge \text{ssthresh}$), la crescita diventa **lineare** (*Additive Increase*): `cwnd` aumenta di circa **$1\text{ MSS}$ per ogni RTT** ($\text{cwnd} \leftarrow \text{cwnd} + \text{MSS} \cdot \frac{\text{MSS}}{\text{cwnd}}$ ad ogni ACK).
3. **Reazione agli Eventi di Perdita (Rilevamento della Congestione):**
   * **Perdita grave (scadenza del Timeout RTO):** la rete è fortemente congestionata. TCP imposta $\text{ssthresh} = \frac{\text{cwnd}}{2}$, azzera $\text{cwnd} = 1\text{ MSS}$ e riparte dallo **Slow Start** (TCP Tahoe / Reno).
   * **Perdita lieve (3 ACK Duplicati – *Fast Recovery* in TCP Reno):** alcuni segmenti continuano comunque ad arrivare al destinatario. TCP esegue la *Fast Retransmit*, dimezza la finestra ($\text{ssthresh} = \frac{\text{cwnd}}{2}$, $\text{cwnd} = \text{ssthresh} + 3\text{ MSS}$) e prosegue direttamente in **Congestion Avoidance** senza ricadere nello Slow Start.