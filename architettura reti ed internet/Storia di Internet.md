---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
argomento: Storia di Internet ed evoluzione delle reti
tags:
  - reti
  - storia-internet
  - networking
  - tcp-ip
  - arpanet
---
# 🌐 Storia di Internet ed Evoluzione delle Reti

## 1. Definizione Ufficiale di Internet
Secondo la risoluzione del **Federal Networking Council (FNC)** del **24 ottobre 1995**, **Internet** è il sistema informativo globale che:
* È logicamente collegato da uno spazio di indirizzamento univoco a livello globale basato sull'**Internet Protocol (IP)** e sue estensioni.
* È in grado di supportare comunicazioni tramite la suite **TCP/IP** (o altri protocolli IP-compatibili).
* Fornisce, utilizza o rende accessibili (pubblicamente o privatamente) servizi di alto livello stratificati sull'infrastruttura di comunicazione.

---

## 2. Le Origini: Dai Mainframe alle Prime Reti (Anni '60)

### L'era dei Mainframe e dei Terminali
Agli inizi dell'informatica, i calcolatori richiedevano enormi investimenti economici ed erano centralizzati:
* **Elaborazione Batch** e collegamento punto-punto tramite **modem** verso terminali remoti.
* **IBM 3270 Information Display System:** architettura gerarchica composta da un **Mainframe** centrale collegato a:
  * *Control Unit locale* (per terminali vicini).
  * *Communication Controller* $\rightarrow$ *Modem* $\rightarrow$ *Control Unit remota* (per terminali distanti).

> [!quote] Aneddoto storico – Vint Cerf (anni '60)
> *"Jon drew all the pictures, Steve and I strung the zucchini and the yellow squash. I think we must have collectively spent about 8 hours on this. Note that this network can't work - there is no mouth/ear link anywhere!!! Such was the state of networking in the primitive 1960s..."* — **Vint Cerf**

---

## 3. Standardizzazione e Modelli di Riferimento

### Modello [[ISO-OSI]] (Open Systems Interconnection)
Modello concettuale sviluppato per standardizzare il modo in cui le reti inviano i dati dal mittente al destinatario, permettendo l'interoperabilità tra sistemi diversi. È strutturato in **7 livelli** (dal basso verso l'alto):

1. **Fisico** (Physical)
2. **Collegamento Dati** (Data Link)
3. **Rete** (Network)
4. **Trasporto** (Transport)
5. **Sessione** (Session)
6. **Presentazione** (Presentation)
7. **Applicazione** (Application)

---

## 4. Evoluzione dell'Infrastruttura e delle Topologie

### Tecnologie di Rete Locale (LAN) Storiche
* **[[Token Ring]]:** metodo di accesso deterministico al mezzo trasmissivo basato sul passaggio di un *token* (gettone) lungo un anello fisico/logico.
  * Sull'anello circolano frame vuoti; chi possiede il token può trasmettere.
  * Il destinatario copia il messaggio e azzera il token; il frame torna infine al mittente.
  * Utilizzava concentratore **MAU** (*Multi-Station Access Unit*) e connettori IBM Type 1 o RJ-45.
* **Workstation Diskless e [[RARP]]:**
  * Le prime stazioni prive di disco rigido caricavano il sistema operativo dalla rete usando il **Reverse Address Resolution Protocol (RARP)**.
  * Invio di una richiesta in broadcast con il proprio **MAC address** per ottenere in risposta da un server il proprio **indirizzo IP**.

---

## 5. Sviluppo del Routing e Interconnessione delle Reti

Con la crescita delle reti interconnesse sono nati i protocolli di routing dinamico per mantenere aggiornate le *routing table*:
* **[[RIP]] (Routing Information Protocol):** definito negli **RFC 1058** e **1023**, implementato dal demone Unix/Linux `routed`. Basato sull'algoritmo *Distance-Vector* (Bellman-Ford), con limite massimo di **15 hop** (16 = infinito/irraggiungibile) e aggiornamenti periodici ogni 30 secondi.
* **[[OSPF]] (Open Shortest Path First):** protocollo *Link-State* più avanzato e veloce a convergere rispetto a RIP. Utilizza pacchetti *Hello* per scoprire i router adiacenti ed eleggere **DR** (*Designated Router*) e **BDR** (*Backup Designated Router*), scambiando *Link-State Advertisement (LSA)* per calcolare il percorso migliore.

---

## 6. Origini dei Contenuti Online (*Origins of Online Content*)
Con l'espansione di Internet, le sorgenti di informazione del mondo reale sono state integrate nella rete tramite diversi meccanismi:
* **Legacy Systems:** integrazione di sistemi informatici preesistenti (basati su tecnologie datate ma ancora fondamentali per affidabilità e sicurezza).
* **Digitalizzazione:** acquisizione di libri e risorse cartacee tramite **scanner e OCR**.
* **Multimedia e Voce:** trasmissione in broadcast su Internet di flussi audio/video e sviluppo del **VoIP** (*Voice over IP*).
* **Dati in tempo reale e ubiquità:** mercati finanziari, *embedded chips* e accesso da qualsiasi luogo (*Anyplace Online!*).