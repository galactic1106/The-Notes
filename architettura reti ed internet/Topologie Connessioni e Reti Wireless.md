---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
argomento: Tipi di connessione, topologie di rete, reti wireless e mesh
tags:
  - reti
  - topologie
  - wireless
  - mesh
  - wifi
  - wimax
---

# 🕸️ Tipi di Connessione, Topologie e Reti Wireless

## 1. Tipi di Connessione
A seconda del numero di terminali (*DTE – Data Terminal Equipment*) coinvolti sul canale, si distinguono due modalità:

* **Point-to-Point (Punto-Punto):** collegamento dedicato tra due soli DTE (PC), ottenuto mediante linea telefonica o linea dedicata.
* **Multi-Point (Multipunto):** collegamento condiviso tra più DTE in cui l'intera capacità del canale deve essere ripartita tra tutti i nodi:
  * *Condivisione temporale:* l'accesso al mezzo trasmissivo è esclusivo e avviene a turni.
  * *Condivisione spaziale:* è ammesso l'uso simultaneo del canale.

---

## 2. Topologia delle Reti
La **topologia** di una rete è la configurazione geometrica dei collegamenti fra i vari componenti. La scelta topologica mira a tre obiettivi principali:
1. **Massima stabilità** (tolleranza ai guasti).
2. **Alto rendimento complessivo**.
3. **Minimizzazione dei costi**.

### Rete ad Albero (*Tree Topology*)
* Dispone le stazioni su **vari livelli gerarchici**, con un nodo/livello centrale di massima importanza (radice/dorsale).
* **Pro:** fornisce una struttura semplice ed espandibile.
* **Contro:** risulta **vulnerabile**, poiché un guasto ai livelli superiori isola interi rami della rete.

---

## 3. Gerarchia delle Reti Wireless (per Estensione)
Le tecnologie wireless si classificano per raggio di copertura dal livello personale a quello geografico:

| Scala | Estensione | Tecnologie e Standard Principali |
| :--- | :--- | :--- |
| **[[PAN]]** | Personal Area Network | **[[Bluetooth]]**, **UWB** (IEEE 802.15), HomeRF. |
| **[[LAN]]** | Local Area Network | **[[Wi-Fi]]** (IEEE 802.11), Wireless Ethernet. |
| **[[MAN]]** | Metropolitan Area Network | **[[WiMAX]]** (IEEE 802.16), **Wi-Fi Mesh** / Mesh Networks. |
| **[[WAN]]** | Wide Area Network | Reti cellulari (400/800/900 MHz, 2.5G, 3G, **HSDPA/HSUPA**, **LTE**), IEEE 802.20. |

---

## 4. Reti Mesh (*Wireless Mesh Networks*)
* **Problematica del canale radio:** il mezzo di trasmissione radio è soggetto a un altissimo tasso di errore, rendendo necessario lo sviluppo di protocolli di routing specificamente ottimizzati per il mondo wireless.
* **Architettura:** la maglia realizzata tra i **wireless router** e gli **access point** forma il **wireless backhaul multihop**.
* **Funzione:** fornisce a ogni utente mobile servizi a basso costo di interconnessione *multihop* a banda larga utilizzando un numero limitato di punti di accesso cablati verso Internet.