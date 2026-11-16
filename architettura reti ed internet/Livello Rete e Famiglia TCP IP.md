---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
argomento: Livello Rete (Network Layer) ISO/OSI e famiglia di protocolli TCP/IP
tags:
  - reti
  - network-layer
  - tcp-ip
  - ip
  - routing
  - router
---

# 🌐 Livello Rete (*Network Layer*) e Famiglia TCP/IP

## 1. Funzioni del Network Layer (Livello 3 ISO/OSI)
Il terzo livello del modello ISO/OSI è responsabile della realizzazione di una **connessione logica end-to-end tra due nodi della rete**: il nodo sorgente e il nodo destinatario.

### Servizi e compiti principali
* **Indirizzamento logico:** identificazione univoca dei nodi nella rete (protocollo **[[IP]]**).
* **Instradamento (*Routing*):** scelta e gestione delle regole che permettono di instradare le informazioni lungo la rete in base all'indirizzo della rete di destinazione.
* **Movimento dei dati e controllo:** gestione del trasferimento dei pacchetti nella sottorete, incluse le procedure di controllo dei flussi.

### Apparati di Livello 3
* **[[Router]]**
* **Switch di Livello 3** (*Layer 3 Switch*): apparati di commutazione abilitati a svolgere funzioni di routing.

---

## 2. La Famiglia di Protocolli [[TCP-IP]]
Nello stack operativo di Internet, i protocolli cooperano su più livelli per garantire indirizzamento, controllo, instradamento e trasporto:

```text
+-----------------------+-----------------------+
|          TCP          |          UDP          |  <-- Trasporto (End-to-End)
+-------+-------+-------+-------+-------+-------+
| ICMP  | IGMP  |  BGP  | OSPF  |  RIP  |       |  <-- Controllo e Routing
+-------+-------+-------+-------+-------+-------+
|                      IP                       |  <-- Indirizzamento e Instradamento
+-----------------------+-----------------------+
|          ARP          |         RARP          |  <-- Risoluzione Indirizzi (L2 <-> L3)
+-----------------------+-----------------------+