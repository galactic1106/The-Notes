---
corso: Architettura Reti ed Internet
modulo: Modulo I
argomento: Controllo di flusso (RTS/CTS, XON/XOFF, Sliding Window) e protocolli ARQ (Stop-and-Wait, Go-Back-N, Selective Repeat)
tags:
  - reti
  - data-link
  - flow-control
  - arq
  - sliding-window
---

# Controllo di Flusso e Protocolli ARQ (Sliding Window)

## 1. Controllo di Flusso (Flow Control)
Il controllo di flusso regola la velocità di invio dei dati da parte del mittente per impedire la saturazione (*overflow*) del buffer di memoria di un ricevitore più lento, indipendentemente dagli errori di linea[cite: 4, 6].

### A. Controllo di Flusso Hardware: RTS / CTS
* **Contesto:** utilizzato nelle comunicazioni seriali punto-punto (standard **RS-232**) tramite linee di controllo dedicate[cite: 4].
* **Funzionamento:**
  1. La stazione trasmittente attiva il segnale **RTS** (*Request to Send*) per chiedere il permesso di trasmettere[cite: 4].
  2. Se il ricevitore ha spazio disponibile nel buffer ed è pronto a ricevere, attiva il segnale **CTS** (*Clear to Send*), dando il via libera all'invio dei dati.
  3. Quando il buffer del ricevitore si avvicina alla saturazione, il segnale CTS viene disattivato per mettere in pausa il mittente.

### B. Controllo di Flusso Software: XON / XOFF
* **Contesto:** non richiede linee fisiche aggiuntive, ma utilizza due speciali caratteri di controllo **ASCII** inseriti direttamente nel flusso dati (*in-band*).
* **Funzionamento:**
  1. Quando il buffer del terminale ricevente si satura, il ricevitore invia al mittente il carattere **XOFF** (*Transmit Off*), bloccando l'emissione di nuovi blocchi di dati.
  2. Non appena il ricevitore elabora i dati e libera spazio in memoria, invia il carattere **XON** (*Transmit On*) per autorizzare la ripresa della trasmissione.
* **Limite:** funziona solo su canali trasparenti ai caratteri di controllo (se i dati binari contengono accidentalmente la sequenza di bit di XOFF, la trasmissione si blocca in modo errato).

### C. Controllo basato su Finestra (*Sliding Window* / Buffer)
* Il ricevitore comunica esplicitamente al mittente quanti frame (o byte) è ancora in grado di accettare in base allo spazio libero nel proprio buffer di ricezione (*finestra del destinatario*).
* Il mittente non può trasmettere oltre il limite indicato dalla finestra finché il ricevitore non invia un riscontro che libera nuove posizioni.

![[M1_04b_Flow_Control.svg]]

---

## 2. Protocolli ARQ (Automatic Repeat reQuest)
Combinano rilevamento errori, riscontri (`ACK`/`NAK`), timer di ritrasmissione e numerazione di sequenza per garantire consegna affidabile su canali rumorosi.

### A. Stop-and-Wait ARQ
* Il mittente trasmette **un solo frame** alla volta (numerato modulo 2: `0` o `1`) e attende l'`ACK` prima di inviare il successivo.
* **Gestione perdite:** se il frame o l'`ACK` vanno persi, scade il **timeout** e il mittente ritrasmette il frame. Il numero di sequenza a 1 bit consente al ricevitore di scartare eventuali frame duplicati.
* **Efficienza (Utilizzazione del canale $U$):**
  $$U = \frac{T_{tx}}{T_{tx} + 2 T_{prop} + T_{proc} + T_{ack}} \approx \frac{1}{1 + 2a}, \quad \text{con } a = \frac{T_{prop}}{T_{tx}}$$
  dove $a = \frac{T_{prop}}{T_{tx}}$ (ritardo di propagazione normalizzato) e $T_{tx} = \frac{L}{R}$ (lunghezza frame / bit-rate) e $T_{prop} = \frac{d}{v}$ (distanza / velocità propagazione). Su canali ad alto prodotto banda-ritardo ($a \gg 1$), l'efficienza crolla.

---

### B. Sliding Window (Finestra Scorrevole)
Consente al mittente di trasmettere fino a $W_s$ frame consecutivi in pipeline prima di ricevere un `ACK`, aumentando drasticamente l'utilizzazione del canale:
$$U = \min\left(1, \frac{W_s}{1 + 2a}\right)$$
* **Piggybacking:** invece di inviare un frame di `ACK` separato, il riscontro viene inserito in un apposito campo dell'header dei frame dati diretti in senso opposto, risparmiando banda.

---

### C. Go-Back-N ARQ
* **Finestra di trasmissione:** $W_s \le 2^n - 1$ (con numeri di sequenza su $n$ bit). *Nota:* non può essere $2^n$ per evitare ambiguità tra frame nuovi e ritrasmissioni in caso di perdita totale di un'intera finestra di `ACK`.
* **Finestra di ricezione:** $W_r = 1$ (accetta solo il frame atteso in ordine $R_{next}$).
* **Funzionamento:**
  * Se un frame arriva fuori sequenza o corrotto, il ricevitore lo scarta (eventualmente invia un `NAK` o rinnova l'`ACK` cumulativo dell'ultimo frame valido).
  * Alla scadenza del timeout del frame più vecchio non riscontrato, il mittente **ritrasmette quel frame e tutti i frame successivi** già inviati nella finestra.
* **Pro/Contro:** ricevitore molto semplice (nessun buffering fuori ordine), ma spreca banda ritrasmettendo anche frame già arrivati integri in caso di alto tasso d'errore.

---

### D. Selective Repeat ARQ
* **Finestre:** $W_s = W_r \le 2^{n-1}$ (metà dello spazio dei numeri di sequenza, per evitare sovrapposizione tra finestra vecchia e nuova).
* **Funzionamento:**
  * Il ricevitore dispone di un buffer per memorizzare i frame integri arrivati **fuori ordine** all'interno della propria finestra $W_r$.
  * Invia un `NAK` selettivo (`SREJ`) per il singolo frame mancante/corrotto (o attende il timer individuale del singolo frame lato mittente).
  * Il mittente ritrasmette **esclusivamente** il frame perso/corrotto. Quando il "buco" viene colmato, il ricevitore consegna in ordine tutti i frame contigui al livello superiore e fa avanzare la finestra.
* **Pro/Contro:** massima efficienza su canali rumorosi o ad alta latenza, ma maggiore complessità di gestione dei buffer e dei timer individuali.

![[M1_04b_protocolli_ARQ.svg]]