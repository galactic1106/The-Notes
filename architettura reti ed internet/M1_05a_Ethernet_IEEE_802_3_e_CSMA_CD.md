---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_e.pdf
argomento: Reti locali LAN, architettura IEEE 802, Ethernet 802.3, formato della trama, CSMA/CD ed evoluzioni
tags:
  - reti
  - ethernet
  - ieee-802-3
  - csma-cd
  - mac-address
  - lan
---

# 🔌 Reti Locali LAN ed Ethernet (IEEE 802.3)

## 1. Architettura dello Standard [[IEEE 802]]
Per le reti locali (**[[LAN]]**) e metropolitane (**[[MAN]]**), il comitato **IEEE 802** suddivide il livello Data Link (Livello 2 ISO/OSI) in due sottolivelli distinti:

* **[[LLC]] (*Logical Link Control* – IEEE 802.2):** sottolivello superiore indipendente dalla tecnologia fisica; gestisce l'interfaccia con il livello Network (multiplexing dei protocolli tramite campi DSAP/SSAP) e fornisce servizi di controllo del flusso e degli errori (senza connessione, con connessione o con riscontro).
* **[[MAC]] (*Medium Access Control*):** sottolivello inferiore dipendente dal mezzo trasmissivo; regola l'accesso al canale condiviso, l'indirizzamento fisico tramite **[[MAC Address]]** a $48\text{ bit}$ ($6\text{ byte}$) e il rilevamento degli errori tramite **FCS** (*Frame Check Sequence*).

---

## 2. Metodo di Accesso [[CSMA-CD]]
Nelle reti **[[Ethernet]]** tradizionali su mezzo condiviso (bus o hub), l'accesso multiplo è regolato dal protocollo **CSMA/CD** (*Carrier Sense Multiple Access with Collision Detection*):

1. **Carrier Sense (Ascolto della portante):** prima di trasmettere, la stazione ascolta il canale; se è libero inizia la trasmissione, altrimenti attende.
2. **Multiple Access (Accesso multiplo):** più stazioni condividono lo stesso mezzo e possono iniziare a trasmettere contemporaneamente se rilevano il canale libero.
3. **Collision Detection (Rilevamento delle collisioni):** durante l'invio, la stazione continua a monitorare il mezzo. Se rileva una sovrapposizione di segnali (collisione):
   * Interrompe immediatamente la trasmissione dei dati.
   * Invia un segnale di disturbo (**Jam Signal** di $32\text{ bit}$) affinché tutte le stazioni si accorgano della collisione.
   * Attende un intervallo di tempo casuale calcolato con l'algoritmo di **Binary Exponential Backoff** prima di ritentare (fino a un massimo di $16$ tentativi).

> [!important] Vincolo sulla dimensione minima del frame
> Affinché una stazione possa rilevare con certezza una collisione prima di aver terminato l'invio del frame, il tempo di trasmissione del frame deve essere almeno pari al tempo di propagazione andata e ritorno (**RTT**, *Round-Trip Time*) lungo il segmento massimo di rete ($2\tau$, noto come *slot time* di $512\text{ bit}$). Da ciò deriva la dimensione minima del frame Ethernet di **$64\text{ byte}$** ($512\text{ bit}$).

---

## 3. Formato della Trama Ethernet (DIX / Ethernet II e IEEE 802.3)

| Campo | Dimensione | Descrizione |
| :--- | :---: | :--- |
| **Preambolo** | $7\text{ byte}$ | Alternanza di bit `10101010` per sincronizzare i clock di trasmettitore e ricevitore. |
| **SFD** (*Start Frame Delimiter*) | $1\text{ byte}$ | Sequenza `10101011` che indica l'inizio effettivo della trama. |
| **Destination MAC Address** | $6\text{ byte}$ | Indirizzo fisico del destinatario (unicast, multicast o broadcast `FF:FF:FF:FF:FF:FF`). |
| **Source MAC Address** | $6\text{ byte}$ | Indirizzo fisico unicast della scheda di rete mittente. |
| **EtherType / Length** | $2\text{ byte}$ | **Ethernet II (DIX):** valori $\ge 1536$ (`0x0600`) indicano il protocollo incapsulato (es. `0x0800` IPv4, `0x0806` ARP, `0x86DD` IPv6).<br>**IEEE 802.3:** valori $\le 1500$ indicano la lunghezza in byte del campo dati (seguito dall'header LLC). |
| **Payload (Dati + Pad)** | $46 - 1500\text{ byte}$ | Dati del livello superiore. Se inferiori a $46\text{ byte}$, viene aggiunto **padding** per raggiungere i $64\text{ byte}$ minimi della trama. |
| **FCS** (*Frame Check Sequence*) | $4\text{ byte}$ | Codice **CRC-32** per il rilevamento degli errori di trasmissione. |

*(Nota: preambolo e SFD vengono gestiti a livello fisico e non sono conteggiati nei $64 - 1518\text{ byte}$ della trama Data Link).*

---

## 4. Evoluzione degli Standard Fisici Ethernet
Nel passaggio dalle architetture condivise (*half-duplex*) a quelle commutate tramite **[[Switch]]** (*full-duplex* su doppino o fibra), il CSMA/CD non è più necessario poiché ogni porta costituisce un dominio di collisione dedicato:

* **10Base5 (*Thick Ethernet*):** cavo coassiale spesso ($50\,\Omega$), topologia a bus, lunghezza max $500\text{ m}$, velocità $10\text{ Mbps}$.
* **10Base2 (*Thin Ethernet*):** cavo coassiale sottile RG-58, connettori BNC, lunghezza max $185\text{ m}$, velocità $10\text{ Mbps}$.
* **10Base-T:** doppino intrecciato UTP Cat. 3/5, connettori RJ-45, topologia a stella (hub/switch), max $100\text{ m}$, $10\text{ Mbps}$.
* **100Base-TX (*Fast Ethernet* – IEEE 802.3u):** UTP Cat. 5, velocità $100\text{ Mbps}$, supporto Full-Duplex e Auto-Negotiation.
* **1000Base-T / SX / LX (*Gigabit Ethernet* – IEEE 802.3z / 802.3ab):** velocità $1\text{ Gbps}$ su doppino UTP Cat. 5e/6 (4 coppie bidirezionali) o fibra ottica multimodale/monomodale.
* **10 Gigabit Ethernet (IEEE 802.3ae) e oltre ($40 / 100\text{ Gbps}$):** esclusivamente **full-duplex** su fibra ottica o doppino Cat. 6a/7 per dorsali e data center.