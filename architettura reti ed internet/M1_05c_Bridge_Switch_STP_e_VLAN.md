---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_e.pdf
argomento: Interconnessione a livello Data Link, Bridge, Switch, MAC Address Table, Spanning Tree Protocol (STP) e VLAN
tags:
  - reti
  - switch
  - bridge
  - stp
  - vlan
  - ieee-802-1q
  - ieee-802-1d
---

# 🔀 Commutazione a Livello 2: Bridge, Switch, STP e VLAN

## 1. Domini di Collisione vs Domini di Broadcast
* **Dominio di collisione:** porzione di rete in cui due o più nodi condividono lo stesso mezzo trasmissivo e possono generare collisioni se trasmettono contemporaneamente.
  * **[[Hub]] (Livello 1):** estende un unico dominio di collisione e un unico dominio di broadcast.
  * **[[Bridge]] e [[Switch]] (Livello 2):** segmentano la rete; **ogni singola porta** costituisce un **dominio di collisione separato**.
* **Dominio di broadcast:** insieme di tutti i nodi che ricevono una trama inviata all'indirizzo broadcast di livello 2 (`FF:FF:FF:FF:FF:FF`).
  * Switch e bridge **inoltrano il traffico broadcast su tutte le porte** (tranne quella di ingresso): mantengono quindi un **unico dominio di broadcast** (a meno di non configurare delle **VLAN**).
  * Solo i **[[Router]] (Livello 3)** separano sia i domini di collisione sia i domini di broadcast.

---

## 2. Funzionamento di [[Bridge]] e [[Switch]] (*Transparent Bridging*)
Gli switch sono bridge multiporta ad alte prestazioni (commutazione in hardware tramite ASIC) che operano in modo trasparente agli host tramite una **MAC Address Table** (o *CAM Table*):

1. **Learning (Apprendimento):** quando uno switch riceve un frame su una porta $P$, legge il **Source MAC Address** e registra l'associazione `(MAC Sorgente -> Porta P)` nella propria tabella con un timer di scadenza (*aging time*).
2. **Forwarding / Filtering (Inoltro o Filtraggio):** lo switch consulta la tabella cercando il **Destination MAC Address**:
   * **Unicast noto su porta diversa da $P$:** inoltra il frame **solo** verso la porta di destinazione (*forwarding*).
   * **Unicast noto sulla stessa porta $P$:** scarta il frame, poiché mittente e destinatario si trovano già sullo stesso segmento (*filtering*).
   * **Unicast sconosciuto (*Unknown Unicast*) o Broadcast/Multicast:** invia il frame su **tutte le porte attive** eccetto quella di provenienza (*flooding*).

### Modalità di Commutazione dello Switch
* **Store-and-Forward:** riceve l'intera trama, verifica il codice **CRC/FCS** per scartare i frame corrotti e poi la inoltra (massima affidabilità, latenza proporzionale alla lunghezza del frame).
* **Cut-Through:** inizia l'inoltro non appena legge i primi $6\text{ byte}$ del Destination MAC Address (latenza minima, ma propaga eventuali frame corrotti).
* **Fragment-Free:** legge i primi $64\text{ byte}$ (dimensione minima Ethernet) per filtrare almeno i frammenti di collisione (*runt frames*) prima di inoltrare.

---

## 3. [[Spanning Tree Protocol]] (STP – IEEE 802.1D)
Nelle reti locali commutate si introducono collegamenti ridondanti tra switch per garantire tolleranza ai guasti. Tuttavia, la ridondanza a livello 2 crea **cicli chiusi (loop)** che causano gravi malfunzionamenti:
* **Broadcast Storm:** trame broadcast o unknown unicast replicate all'infinito tra gli switch fino a saturare completamente la banda.
* **Instabilità della MAC Table:** lo stesso MAC sorgente viene appreso continuamente su porte diverse.
* **Copie multiple dei frame** consegnate al destinatario.

### Come Risolve il Problema l'STP
Il protocollo **STP (IEEE 802.1D)** costruisce logicamente una topologia **ad albero priva di cicli** disabilitando logicamente i collegamenti ridondanti (che restano in standby pronti a riattivarsi automaticamente in caso di guasto):

1. **Elezione del Root Bridge:** gli switch si scambiano messaggi **BPDU** (*Bridge Protocol Data Unit*); viene eletto come radice dell'albero lo switch con il **Bridge ID** più basso ($\text{Bridge ID} = \text{Priorità} + \text{MAC Address}$).
2. **Selezione delle Root Port (RP):** su ogni switch non-root, viene scelta la singola porta con il **costo di percorso minimo** verso il Root Bridge.
3. **Selezione delle Designated Port (DP):** su ogni segmento di rete, la porta che offre il percorso migliore verso il Root Bridge diventa Designated Port (stato *Forwarding*).
4. **Blocco delle porte rimanenti:** tutte le altre porte ridondanti vengono poste in stato **Blocking** (non inoltrano traffico dati, ma continuano ad ascoltare le BPDU per rilevare eventuali guasti).

*(Nota: la versione evoluta **RSTP – IEEE 802.1w** riduce i tempi di convergenza da $30 - 50\text{ secondi}$ a pochi millisecondi).*

---

## 4. Reti Locali Virtuali: [[VLAN]] (IEEE 802.1Q)
Una **VLAN** (*Virtual LAN*) consente di segmentare logicamente un'infrastruttura fisica condivisa in più reti locali indipendenti:

* **Isolamento:** ogni VLAN corrisponde a un **dominio di broadcast separato** e a una diversa sottorete IP; il traffico tra VLAN diverse può passare solo attraverso un dispositivo di livello 3 (Router o Layer 3 Switch, *Inter-VLAN Routing*).
* **Porte Access vs Porte Trunk:**
  * **Access Port:** appartiene a una singola VLAN e collega dispositivi terminali (PC, stampanti) che inviano traffico Ethernet standard (*untagged*).
  * **Trunk Port:** collega due switch (o uno switch e un router) trasportando simultaneamente il traffico di più VLAN sullo stesso collegamento fisico.
* **Tagging IEEE 802.1Q:** sulle porte trunk, lo switch inserisce nell'header Ethernet un campo aggiuntivo di **$4\text{ byte}$** (`TPID = 0x8100` + `TCI`) contenente il **VLAN ID (VID)** su $12\text{ bit}$ (fino a $4094$ VLAN utilizzabili) e $3\text{ bit}$ di priorità QoS (IEEE 802.1p).