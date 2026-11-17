---
corso: Architettura Reti ed Internet
modulo: Modulo II - Prof. Damiano Perri
fonte: parte_1.pdf
argomento: Programma Modulo 2, modalità d'esame, teoria dell'informazione e protocolli
tags:
  - reti
  - modulo-2
  - perri
  - teoria-informazione
  - protocolli
  - dikw
---

# 📡 Introduzione al Modulo II e Fondamenti della Comunicazione

## 1. Struttura del Corso e Modalità d'Esame (Modulo II)
* **Docente:** Prof. Damiano Perri (Dipartimento di Matematica e Informatica, UNIPG).
* **Obiettivi:** comprendere le tipologie di reti telematiche, la trasmissione dei dati, i protocolli di rete, la configurazione degli apparati di instradamento/servizi e i sistemi di controllo, monitoraggio e sicurezza.
* **Modalità d'esame:**
  * **Prova scritta** unica in comune tra Modulo I (Gervasi) e Modulo II (Perri).
  * **Prova orale.**
  * Le dispense del docente sono sufficienti per superare l'esame; per approfondimenti si consigliano *Reti di calcolatori* (Bruno Checcucci) e *TCP/IP* (Gary Govanus), oltre ai portali accademici (IEEE Xplore, ACM, Springer Link, Google Scholar) accessibili tramite **VPN universitaria**.

### Sintesi del Programma del Modulo II
1. **Prima Parte (Trasmissione e Livello 2):** misura dell'informazione, topologie, protocolli, codici e gestione dell'errore, trasmissione sincrona/asincrona, segnali analogici e digitali, modulazione, multiplazione, modem, interfacce, reti commutate/dedicate, livello Data Link (BSC, HDLC, SDLC, PPP).
2. **Seconda Parte (Livello 3, Servizi e Sicurezza):** protocollo IP, sottoreti e interfacce, routing (minimale, statico, dinamico), DNS e Host table, posta elettronica (`sendmail`, `aliases`), monitoraggio e sicurezza (analisi dei rischi, TCP wrapper, Firewall).

---

## 2. Comunicazione Dati e Modello di [[Shannon-Weaver]]
La **comunicazione dati** consiste nello scambio di informazione tra due o più dispositivi attraverso un **mezzo trasmissivo** mediante un **sistema di comunicazione** (hardware e software) composto da:
1. **Sorgente di informazione** (trasmettitore).
2. **Mezzo trasmissivo** (canale fisico).
3. **Destinatario** (ricevitore).

### Schema del Processo Comunicativo
1. La **Fonte** codifica il messaggio secondo un **codice** condiviso.
2. Il segnale attraversa il **canale**, dove può essere alterato dal **rumore** (*noise*).
3. Il **Destinatario** decodifica il segnale per ricostruire il messaggio (con eventuale canale di ritorno).

### La Piramide [[DIKW]]
Dal livello più grezzo a quello decisionale:
1. **Dati (*Data*):** elementi grezzi e oggettivi privi di contesto.
2. **Informazione (*Information*):** dati correlati, organizzati e dotati di contesto.
3. **Conoscenza (*Knowledge*):** rielaborazione soggettiva e strutturata delle informazioni.
4. **Saggezza (*Wisdom*):** capacità applicativa e decisionale.

---

## 3. Misura dell'Informazione
L'informazione è una grandezza misurabile la cui unità fondamentale è il **bit** (*binary digit*). Dato un sistema che può assumere **$M$ stati possibili ed equiprobabili**, il numero di bit **$Q$** necessari per distinguerli univocamente è:

$$Q = \log_2(M)$$

* Finalità ultima di un sistema di comunicazione è il trasferimento dell'informazione tra sistemi remoti sotto il controllo di programmi applicativi.

---

## 4. Il Protocollo di Comunicazione
Un **protocollo** è l'insieme di regole formali che governano la comunicazione tra due o più entità definendo:
* **Sintassi:** formato e struttura dei dati.
* **Semantica:** significato dei campi e azioni da intraprendere.
* **Temporizzazione (*Timing*):** tempi e velocità di invio.

> [!note] Necessità del protocollo
> Due dispositivi possono essere fisicamente interconnessi, ma senza un protocollo condiviso non sono in grado di comunicare.