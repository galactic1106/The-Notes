---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_b.pdf
argomento: Comunicazione dati, modello di Shannon-Weaver, piramide DIKW, misura dell'informazione e protocolli
tags:
  - reti
  - teoria-informazione
  - shannon
  - dikw
  - bit
  - protocolli
---

# 📡 Teoria dell'Informazione, Misura e Protocolli

## 1. Cos'è la Comunicazione Dati
Per **comunicazione dati** si intende lo scambio di informazione tra due o più dispositivi attraverso un **mezzo trasmissivo**. Un **sistema di comunicazione** (hardware + software) richiede tre componenti fondamentali:
1. **Sorgente di informazione** (dispositivo trasmittente).
2. **Mezzo trasmissivo** (cammino fisico lungo cui viaggia il segnale).
3. **Destinatario** (dispositivo ricevente).

---

## 2. Teoria dell'Informazione e Modello [[Shannon-Weaver]]
* **Informazione:** insieme di dati correlati tra loro con cui un'idea o un fatto prende forma ed è comunicato.
* **Comunicazione** *(dal latino cum = con, munire = legare/costruire e communico = mettere in comune)*: processo di trasmissione di un'informazione tramite lo scambio di un **messaggio** elaborato secondo le regole di un **codice** condiviso.

### Schema del Processo Comunicativo
1. La **Fonte** applica una **codifica** al messaggio trasformandolo in segnale.
2. Il segnale attraversa il **canale trasmissivo**, dove può subire alterazioni dovute al **rumore** (*noise*).
3. Il **Destinatario** applica la **decodifica** per ricostruire il messaggio originale (con eventuale canale di feedback/ritorno).

### La Piramide [[DIKW]] (*Data, Information, Knowledge, Wisdom*)
Rappresenta la gerarchia del valore informativo dal basso verso l'alto:
1. **Dati (*Data*):** simboli o valori grezzi, oggettivi e privi di contesto (volume massimo).
2. **Informazione (*Information*):** dati organizzati, strutturati e contestualizzati.
3. **Conoscenza (*Knowledge*):** rielaborazione soggettiva dell'informazione tramite esperienza e regole.
4. **Saggezza (*Wisdom*):** capacità decisionale e applicativa al vertice della piramide.

---

## 3. Misura dell'Informazione
L'informazione è una grandezza misurabile la cui unità fondamentale è il **bit** (*binary digit*). Dato un sistema che può assumere **$M$ stati (o simboli) possibili ed equiprobabili**, la quantità di informazione **$Q$** espressa in bit necessaria per individuare univocamente uno stato è:

$$Q = \log_2(M)$$

* Con $1\text{ bit}$ si distinguono $M = 2^1 = 2$ stati ($0$ o $1$).
* Con $8\text{ bit}$ ($1\text{ Byte}$) si distinguono $M = 2^8 = 256$ stati diversi.

---

## 4. Il Protocollo di Comunicazione
Un **protocollo** è l'insieme di regole formali che disciplinano la comunicazione tra due o più entità:
* **Sintassi:** struttura e formato dei dati (ordine dei bit/byte nei campi del messaggio).
* **Semantica:** significato di ciascuna sezione di bit e azioni da intraprendere di conseguenza.
* **Temporizzazione (*Timing*):** quando i dati devono essere inviati e a quale velocità.

> *Senza un protocollo comune, due dispositivi possono essere fisicamente collegati ma non essere in grado di comprendersi (come due persone che parlano lingue diverse).*