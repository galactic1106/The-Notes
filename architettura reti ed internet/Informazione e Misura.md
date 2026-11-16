---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
argomento: Teoria dell'informazione, misura dell'informazione e protocolli
tags:
  - reti
  - teoria-informazione
  - shannon
  - bit
  - protocolli
---

# 📡 Teoria dell'Informazione e sua Misura

## 1. Cos'è la Comunicazione Dati
Per **comunicazione dati** si intende lo scambio di informazione tra due (o più) dispositivi attraverso l'utilizzo di un **mezzo trasmissivo**. Affinché avvenga, è necessario un **sistema di comunicazione** (composto da hardware e software) formato da tre elementi essenziali:
1. **Sorgente di informazione** (dispositivo trasmittente).
2. **Mezzo trasmissivo** (cammino fisico che consente l'invio dell'informazione).
3. **Destinatario** (dispositivo ricevente).

---

## 2. Teoria dell'Informazione e Modello [[Shannon-Weaver]]
* **Informazione:** insieme di dati correlati tra loro con cui un'idea o un fatto prende forma ed è comunicata.
* **Comunicazione** *(dal latino cum = con, munire = legare/costruire e communico = mettere in comune)*: processo e modalità di trasmissione di un'informazione da un individuo/luogo a un altro tramite lo scambio di un **messaggio** elaborato secondo le regole di un determinato **codice**.

### Il Processo Comunicativo (Shannon-Weaver)
1. La **Fonte** applica una **codifica** al messaggio e lo immette sul canale.
2. Lungo il canale trasmissivo il segnale può essere alterato dal **rumore** (*noise*).
3. Il **Destinatario** riceve il segnale e applica la **decodifica** per ricostruire il messaggio (e viceversa nel canale di ritorno).

### La Piramide [[DIKW]] (*Data, Information, Knowledge, Wisdom*)
Dal basso verso l'alto:
1. **Dati** (*Data*) – base oggettiva, massimo volume.
2. **Informazione** (*Information*) – dati strutturati e contestualizzati.
3. **Conoscenza** (*Knowledge*) – soggettività, valore e struttura applicata.
4. **Saggezza** (*Wisdom*) – vertice decisionale.

---

## 3. Misura dell'Informazione
L'informazione è una grandezza misurabile la cui unità di misura fondamentale è il **bit** (*binary digit*).

$$Q = \log_2(M)$$

* **$M$**: numero degli stati (o simboli) possibili ed equiprobabili di un dato sistema.
* **$Q$**: numero di bit necessari per distinguere univocamente tali stati.

---

## 4. Il Protocollo di Comunicazione
Un **protocollo** è l'insieme di regole che consentono la comunicazione tra due o più dispositivi:
* Definisce **cosa** si sta scambiando (sintassi e semantica dei messaggi).
* Definisce **come** avviene lo scambio (temporizzazione e gestione degli errori).
* *Nota:* senza un protocollo condiviso, due dispositivi possono essere fisicamente connessi ma non riuscire comunque a comunicare.