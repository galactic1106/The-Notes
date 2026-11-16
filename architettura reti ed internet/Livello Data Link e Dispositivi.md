---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
argomento: Livello Data Link ISO/OSI, servizi, dispositivi e domini di collisione
tags:
  - reti
  - data-link
  - ethernet
  - switch
  - bridge
  - mac-address
---

# 🔗 Livello Data Link (Livello 2 ISO/OSI)

## 1. Ruolo del Data Link Layer
Il secondo livello del modello ISO/OSI descrive come un dispositivo **guadagna l'accesso al mezzo trasmissivo** specificato nel livello fisico (*Physical Layer*) e come realizza la **comunicazione affidabile con un nodo adiacente**.
* Definisce il formato dei dati (**frame**).
* Gestisce la frammentazione dei dati all'interno di un messaggio trasmesso.
* Implementa le procedure di controllo dell'errore e dell'accesso al canale.

---

## 2. Servizi del Data Link Layer
I sei servizi fondamentali erogati dal livello Data Link sono:

1. **Framing & Link Access:** incapsulamento dei pacchetti di rete in trame (*frame*) e regolazione dell'accesso al mezzo condiviso.
2. **Reliable Delivery:** consegna affidabile dei frame tra nodi direttamente connessi.
3. **Flow Control:** controllo del flusso per evitare che un trasmettitore veloce saturi un ricevitore più lento.
4. **Error Detection:** rilevazione di eventuali bit alterati durante la trasmissione (es. parità, CRC).
5. **Error Correction:** correzione degli errori rilevati (tramite ritrasmissione o codici autocorrettori).
6. **Half-Duplex & Full-Duplex:** gestione delle modalità di trasmissione bidirezionale alternata (*half-duplex*) o simultanea (*full-duplex*).

---

## 3. Apparati di Rete e Domini di Collisione
Appartengono al livello Data Link (e alla transizione con il livello fisico) diversi dispositivi chiave:

* **[[NIC]] (*Network Interface Card*):** la scheda di rete installata sul dispositivo terminale, dotata di indirizzo fisico univoco (**[[MAC Address]]**).
* **[[Hub]]:** concentratore che replica il segnale su tutte le porte; non separa il traffico e mantiene un **unico dominio di collisione** condiviso.
* **[[Bridge]]:** apparato che collega due segmenti di rete e filtra il traffico in base al **MAC address**, suddividendo il **dominio di collisione Ethernet**.
* **[[Switch]]:** evoluzione multiporta del bridge; opera la commutazione a livello 2 dividendo ogni porta in un **dominio di collisione dedicato** e inoltrando i frame solo verso il destinatario (in base alla tabella degli indirizzi MAC).