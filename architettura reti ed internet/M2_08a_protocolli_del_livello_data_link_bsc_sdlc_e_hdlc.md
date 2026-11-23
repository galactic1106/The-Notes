---
corso: Architettura Reti ed Internet
modulo: Modulo 2 - Prof. Damiano Perri
università: Università degli Studi di Perugia (UNIPG)
fonte: parte_8.pdf
argomento: Protocolli del secondo livello ISO-OSI orientati al carattere (BSC) e al bit (SDLC, HDLC)
tags:
  - reti
  - data-link
  - bsc
  - hdlc
  - sdlc
  - framing
---

# 🔗 Protocolli del Livello Data Link: BSC, SDLC e HDLC

## 1. Classificazione dei Protocolli Data Link
I protocolli del livello di collegamento dati (Livello 2 ISO/OSI) disciplinano il formato delle trame (*frame*), la sincronizzazione tra le stazioni e il controllo di flusso e di errore. Si suddividono in due famiglie storiche e strutturali:

1. **Protocolli orientati al carattere (*Byte-Oriented* / *Character-Oriented*):**
   * Interpretano il flusso trasmesso come una sequenza di caratteri codificati (es. ASCII o EBCDIC).
   * Utilizzano caratteri di controllo dedicati per delimitare l'inizio e la fine del messaggio e per gestire il dialogo (es. **[[BSC]]**).
2. **Protocolli orientati al bit (*Bit-Oriented*):**
   * Trattano i dati come una sequenza arbitraria di bit, indipendente da qualsiasi codifica di caratteri.
   * Impiegano una sequenza fissa di bit (**Flag**) come delimitatore e la tecnica del **Bit Stuffing** per garantire la trasparenza dei dati (es. **[[SDLC]]**, **[[HDLC]]**, **[[PPP]]**).

---

## 2. [[BSC]] (*Binary Synchronous Communications*)
Sviluppato da IBM negli anni '60, il **BSC** (o *Bisync*) è un protocollo **sincrono**, **half-duplex** e **orientato al carattere**, progettato per collegamenti punto-punto e multipunto (*polling/selecting*).

### Caratteri di Controllo Principali
* **`SYN` (*Synchronous Idle*):** almeno due caratteri `SYN` precedono ogni trasmissione per sincronizzare il clock del ricevitore sul flusso di bit.
* **`SOH` (*Start of Header*):** indica l'inizio dell'intestazione di controllo (indirizzi, sequenza).
* **`STX` (*Start of Text*):** segnala la fine dell'header e l'inizio del campo dati (*payload*).
* **`ETX` (*End of Text*) / `ETB` (*End of Transmission Block*):** indicano rispettivamente la fine dell'intero messaggio o di un singolo blocco intermedio.
* **`BCC` (*Block Check Character*):** campo posto dopo `ETX`/`ETB` contenente il codice di controllo degli errori (LRC o CRC).
* **`ENQ` (*Enquiry*), `ACK0`/`ACK1` (riscontro alternato pari/dispari), `NAK` (*Negative Acknowledge*) e `EOT` (*End of Transmission*):** governano le fasi di instaurazione, conferma e chiusura del dialogo.

### Trasparenza dei Dati (*Character / Byte Stuffing*)
Per evitare che un carattere di controllo presente nei dati binari venga interpretato erroneamente come fine trama, BSC utilizza il carattere **`DLE` (*Data Link Escape*)**:
* La modalità trasparente viene avviata con la coppia **`DLE STX`** e terminata con **`DLE ETX`**.
* Se il byte corrispondente a `DLE` compare all'interno del campo dati, il trasmettitore inserisce un secondo `DLE` (**`DLE DLE`**); il ricevitore rimuove il duplicato ripristinando il dato originale.

> [!warning] Limiti del protocollo BSC
> Essendo *half-duplex* e basato sul meccanismo *Stop-and-Wait* (con riscontri alternati `ACK0` / `ACK1`), il BSC presenta una bassa efficienza su collegamenti con elevato ritardo di propagazione ed è dipendente dalla codifica dei caratteri.

---

## 3. [[SDLC]] e [[HDLC]] (*High-Level Data Link Control*)
Per superare i limiti dei protocolli orientati al carattere, IBM introdusse **SDLC** (*Synchronous Data Link Control*), successivamente standardizzato dall'ISO come **HDLC** (ISO 3309 / ISO 4335). È un protocollo **sincrono**, **full-duplex** e **orientato al bit**.

### Tipologie di Stazioni e Modalità Operative HDLC
* **Stazioni:**
  * *Primary (Primaria):* controlla il collegamento ed emette **comandi** (*Commands*).
  * *Secondary (Secondaria):* opera sotto il controllo della primaria ed emette **risposte** (*Responses*).
  * *Combined (Combinata):* integra entrambe le funzioni, potendo emettere sia comandi sia risposte (comunicazione paritetica).
* **Modalità di trasferimento:**
  1. **NRM (*Normal Response Mode*):** configurazione sbilanciata (1 primaria, 1 o più secondarie); la secondaria può trasmettere solo se autorizzata (*polled*) dalla primaria.
  2. **ARM (*Asynchronous Response Mode*):** configurazione sbilanciata in cui la secondaria può avviare trasmissioni senza attendere il permesso esplicito della primaria.
  3. **ABM (*Asynchronous Balanced Mode*):** configurazione bilanciata punto-punto tra due stazioni **combinate**, in cui ciascuna può iniziare la trasmissione autonomamente (modalità più diffusa).

### Struttura della Trama HDLC
```text
+----------+---------+---------+----------------------+---------+----------+
|   Flag   | Address | Control |     Information      |   FCS   |   Flag   |
|  8 bit   | 8+ bit  | 8/16bit |   Lunghezza var.     | 16/32b  |  8 bit   |
+----------+---------+---------+----------------------+---------+----------+
 01111110                                                         01111110
```

1. **Flag (`01111110` / `0x7E`):** delimita l'inizio e la fine della trama. Per evitare falsi flag nei dati si applica il **Bit Stuffing** (ogni volta che nei dati compaiono cinque bit `1` consecutivi, il mittente inserisce automaticamente uno `0`, che il ricevitore rimuove).
2. **Address:** identifica la stazione secondaria (in NRM/ARM) oppure distingue comandi e risposte (in ABM).
3. **Control:** determina la tipologia di trama (8 bit nella modalità base, 16 bit nella modalità estesa per finestre scorrevoli più ampie):
   * **I-Frame (*Information Frame*):** trasporta i dati utente e i numeri di sequenza $N(S)$ (trama inviata) e $N(R)$ (*piggybacking* dell'ACK per la prossima trama attesa).
   * **S-Frame (*Supervisory Frame*):** gestisce il controllo di flusso e di errore quando non ci sono dati da inviare (`RR` *Receive Ready*, `RNR` *Receive Not Ready*, `REJ` *Reject* per Go-Back-N, `SREJ` *Selective Reject*).
   * **U-Frame (*Unnumbered Frame*):** usato per il controllo del collegamento (instaurazione e rilascio sessione, es. `SABM`, `DISC`, `UA`).
4. **Information:** campo dati di lunghezza variabile (presente solo negli I-Frame e in alcuni U-Frame).
5. **FCS (*Frame Check Sequence*):** codice **CRC** a 16 bit (o 32 bit) per la rilevazione degli errori su tutta la trama (esclusi i flag).