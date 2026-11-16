# 04a — Livello Data Link: Funzioni, Framing e Rilevamento Errori

## 1. Ruolo del Livello Data Link (Livello 2 ISO/OSI)
Il livello Data Link si occupa del trasferimento affidabile di informazioni (organizzate in **frame** o trame) tra due nodi adiacenti collegati direttamente da un canale fisico[cite: 4].
* **Interfaccia superiore:** riceve pacchetti dal livello Network e li incapsula in frame[cite: 4].
* **Interfaccia inferiore:** consegna i bit al livello Physical per la trasmissione sul mezzo[cite: 4].
* **Dispositivi tipici:** NIC (schede di rete), Bridge, Switch[cite: 4].
* **Sottolivelli (standard IEEE 802):**
  * **LLC (Logical Link Control):** interfaccia verso il livello Network, gestione del flusso e degli errori[cite: 4].
  * **MAC (Media Access Control):** gestione dell'accesso al mezzo condiviso e indirizzamento fisico (indirizzi MAC a 48 bit)[cite: 4].

---

## 2. Servizi offerti al livello Network
1. **Unacknowledged connectionless service:** il mittente invia frame indipendenti senza instaurare connessione e senza attendere riscontri (ACK). Usato su canali a basso tasso di errore (es. Ethernet) o per traffico real-time.
2. **Acknowledged connectionless service:** nessuna connessione logica instaurata, ma ogni singolo frame viene riscontrato (ACK); se l'ACK non arriva entro un timeout, il frame viene ritrasmesso (es. Wi-Fi IEEE 802.11).
3. **Acknowledged connection-oriented service:** instaurazione della connessione, numerazione dei frame, consegna garantita in ordine e rilascio della connessione (usato su tratte geografiche o collegamenti inaffidabili).

---

## 3. Framing (Delimitazione dei Frame)
Il livello fisico trasmette un flusso continuo di bit. Il Data Link deve suddividerlo in frame discreti riconoscibili dal ricevitore:

* **Conteggio dei caratteri (Character Count):** un campo nell'header indica il numero di byte del frame. *Difetto:* un singolo errore di trasmissione su tale campo desincronizza l'intero flusso successivo.
* **Byte Stuffing (Character Stuffing):** delimita il frame con byte speciali di inizio e fine (`FLAG`, es. `0x7E` o sequenze `DLE STX` / `DLE ETX`). Se il byte `FLAG` compare nei dati, il mittente inserisce davanti un carattere di escape (`ESC` o `DLE`), che il ricevitore rimuove.
* **Bit Stuffing:** delimita inizio e fine frame con il pattern binario `01111110` (`0x7E`). Per evitare falsi flag nel payload, ogni volta che il mittente incontra **cinque `1` consecutivi** nei dati, inserisce automaticamente uno `0` (`0111110...`). Il ricevitore rimuove lo `0` che segue cinque `1` consecutivi.
* **Violazioni della codifica fisica:** utilizzo di transizioni di segnale non valide per i dati (es. nella codifica Manchester o 4B/5B) per marcare i confini del frame.

---

## 4. Controllo degli Errori: Rilevamento e Correzione
Gli errori possono essere **isolati** (singolo bit invertito) o **a raffica (burst)** (sequenza contigua di bit alterati).

### Codici a Rilevamento d'Errore (Error Detection)
* **Bit di parità:** aggiunge 1 bit per rendere pari (o dispari) il numero totale di `1`. Rileva tutti gli errori su un numero dispari di bit, ma fallisce su errori pari.
* **Parità bidimensionale (LRC/VRC):** organizza i dati in matrice con parità per riga e per colonna; rileva errori multipli e corregge singoli errori.
* **Checksum (Somma di controllo):** suddivide i dati in parole da $k$ bit (es. 16 bit), le somma in complemento a 1 e trasmette il complemento del risultato. Leggero da calcolare via software, ma meno robusto del CRC.
* **CRC (Cyclic Redundancy Check):** tratta i bit come coefficienti di un polinomio $M(x)$ su $GF(2)$.
  1. Si sceglie un polinomio generatore $G(x)$ di grado $r$ noto a mittente e ricevitore.
  2. Si appendono $r$ zeri al messaggio ($x^r M(x)$) e si calcola il resto $R(x)$ della divisione modulo 2 per $G(x)$.
  3. Si trasmette $T(x) = x^r M(x) + R(x)$, che è divisibile esattamente per $G(x)$.
  4. Il ricevitore divide il frame ricevuto per $G(x)$: se il resto è $\neq 0$, si è verificato un errore.

### Codici a Correzione d'Errore (FEC — Forward Error Correction)
* **Distanza di Hamming $d(x, y)$:** numero di posizioni in cui due parole di codice differiscono.
  * Per **rilevare** fino a $d$ errori serve un codice con distanza minima $d_{min} \ge d + 1$.
  * Per **correggere** fino a $t$ errori serve un codice con distanza minima $d_{min} \ge 2t + 1$.
* **Codice di Hamming:** inserisce bit di parità nelle posizioni potenza di 2 ($1, 2, 4, 8, \dots$); la somma delle posizioni con parità errata (sindrome) indica direttamente l'indice del bit da invertire per correggere un singolo errore.