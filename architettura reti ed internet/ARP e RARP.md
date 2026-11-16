---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
argomento: Risoluzione degli indirizzi con ARP e RARP
tags:
  - reti
  - arp
  - rarp
  - mac-address
  - ip
  - ethernet
---

# 🔄 Risoluzione degli Indirizzi: ARP e RARP

## 1. Il Problema dell'Incapsulamento (Livello 3 $\rightarrow$ Livello 2)
Nel modello ISO/OSI, quando un pacchetto di livello 3 (**IP**) deve essere incapsulato in un frame di livello 2 (**Data Link**, tipicamente [[Ethernet]]), l'header del frame deve contenere l'**indirizzo fisico (MAC address)** del destinatario locale.

* **Indipendenza degli indirizzi:** gli indirizzi fisici Ethernet vengono preassegnati dai produttori hardware alle singole schede di rete (**[[NIC]]**) e non sono deducibili matematicamente dall'indirizzo IP *(nota: pur essendo cablati sulla scheda, gli indirizzi MAC possono comunque essere alterati via software da utenti esperti)*.
* **Regola di comunicazione locale:** un host può inviare dati sulla rete locale solo conoscendo l'indirizzo MAC:
  * **Dell'host destinatario**, se quest'ultimo appartiene alla **stessa sottorete** del mittente.
  * **Del default gateway (router)**, se l'host destinatario appartiene a una **rete remota**.

---

## 2. [[ARP]] (*Address Resolution Protocol*)
Il protocollo **ARP** risolve dinamicamente la corrispondenza da **Indirizzo IP $\rightarrow$ Indirizzo MAC** all'interno di una rete locale:

1. **ARP Request (Broadcast):** l'host mittente invia un frame in broadcast (a tutti i nodi della LAN) chiedendo *"Chi possiede l'indirizzo IP X?"*.
2. **ARP Reply (Unicast):** solo l'host (o il gateway) che possiede l'IP richiesto risponde direttamente al mittente comunicando il proprio indirizzo fisico (**MAC**).
3. **ARP Cache:** la coppia `(IP, MAC)` viene salvata temporaneamente in una tabella locale (*ARP Table / Cache*) per evitare richieste ripetute ad ogni pacchetto.

---

## 3. [[RARP]] (*Reverse Address Resolution Protocol*)
Il protocollo **RARP** esegue l'operazione inversa, risolvendo la corrispondenza da **Indirizzo MAC $\rightarrow$ Indirizzo IP**:

* **Contesto d'uso:** veniva impiegato dalle prime **workstation *diskless*** (prive di disco rigido locale) che non potevano memorizzare una propria configurazione IP permanente.
* **Funzionamento:** all'avvio, la stazione invia una richiesta in broadcast contenente il proprio **MAC address**; un **RARP Server** sulla rete consulta la propria tabella di configurazione e restituisce alla macchina l'indirizzo **IP** assegnato. *(Oggi sostituito da protocolli più evoluti come BOOTP e [[DHCP]]).*