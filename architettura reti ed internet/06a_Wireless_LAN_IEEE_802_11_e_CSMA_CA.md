---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_f.pdf
argomento: Reti locali wireless WLAN, architettura IEEE 802.11, CSMA/CA, terminale nascosto/esposto e trama 802.11
tags:
  - reti
  - wifi
  - ieee-802-11
  - csma-ca
  - wlan
  - data-link
---

# 📶 Reti Wireless LAN: Standard IEEE 802.11 e CSMA/CA

## 1. Architettura delle Reti [[IEEE 802.11]] (Wi-Fi)
Lo standard **IEEE 802.11** definisce il livello Fisico e il sottolivello MAC per le reti locali senza fili (**[[WLAN]]**). Le stazioni wireless (**STA**) possono organizzarsi in due modalità architetturali principali:

* **Modalità Infrastruttura:**
  * **[[BSS]] (*Basic Service Set*):** cella radio base costituita da un **[[Access Point]] (AP)** e dalle stazioni client associate entro il suo raggio di copertura. L'AP funge da bridge tra il mezzo radio e la rete cablata.
  * **[[ESS]] (*Extended Service Set*):** insieme di più BSS interconnesse tramite un **Distribution System (DS)** (tipicamente una rete Ethernet cablata). L'ESS appare ai livelli superiori come un'unica sottorete logica identificata dallo stesso **[[SSID]]** (*Service Set Identifier*), consentendo il *roaming* tra celle adiacenti.
* **Modalità Ad-Hoc ([[IBSS]] – *Independent BSS*):** rete *peer-to-peer* priva di Access Point, in cui i dispositivi comunicano direttamente tra loro in modo temporaneo.

---

## 2. Problematiche del Mezzo Radio
A differenza del cavo Ethernet, nelle trasmissioni radio non è possibile utilizzare il rilevamento delle collisioni (**CSMA/CD**) per due motivi fondamentali:
1. **Impossibilità di ascoltare mentre si trasmette:** la potenza del segnale trasmesso localmente sovrasta di diversi ordini di grandezza eventuali segnali in arrivo da nodi remoti, rendendo impossibile rilevare una collisione durante l'invio (*half-duplex* intrinseco).
2. **Attenuazione e visibilità parziale:** non tutte le stazioni di una cella si "sentono" reciprocamente.

### Terminale Nascosto vs Terminale Esposto
* **Problema del Terminale Nascosto (*Hidden Terminal*):** due stazioni $A$ e $C$ sono entrambe nel raggio d'azione dell'Access Point $B$, ma troppo distanti tra loro per sentirsi. Se $A$ trasmette verso $B$, $C$ rileva il canale libero e inizia a trasmettere a sua volta, causando una **collisione** presso $B$.
* **Problema del Terminale Esposto (*Exposed Terminal*):** la stazione $B$ sta trasmettendo verso $A$; la stazione $C$ (vicina a $B$, ma lontana da $A$) vorrebbe trasmettere verso $D$. Sentendo il canale occupato da $B$, $C$ rinuncia inutilmente a trasmettere, anche se la sua trasmissione verso $D$ non interferirebbe con la ricezione di $A$.

---

## 3. Protocollo di Accesso [[CSMA-CA]]
Per prevenire le collisioni invece di rilevarle, lo standard 802.11 adotta il protocollo **CSMA/CA** (*Carrier Sense Multiple Access with Collision Avoidance*), parte della funzione **DCF** (*Distributed Coordination Function*):

### Spazi Inter-Trama ([[IFS]] – *Inter-Frame Space*)
Per gestire le priorità di accesso al canale, le stazioni devono attendere intervalli di silenzio di diversa durata:
* **SIFS (*Short IFS*):** intervallo più breve (massima priorità); usato per trame di controllo immediate come **ACK**, **CTS** e frammenti successivi.
* **PIFS (*PCF IFS*):** intervallo intermedio usato in modalità centralizzata (*Point Coordination Function*).
* **DIFS (*Distributed IFS*):** intervallo standard ($\text{DIFS} = \text{SIFS} + 2 \times \text{Slot Time}$) che una stazione deve attendere prima di iniziare una nuova trasmissione dati.

### Procedura di Trasmissione e Riscontro (ACK)
1. La stazione ascolta il canale (**Physical & Virtual Carrier Sense**).
2. Se il canale è libero per un tempo pari a **DIFS**, avvia la trasmissione (oppure attende un **Contention Window / Backoff casuale** se il canale era precedentemente occupato).
3. Poiché il mittente non può rilevare collisioni durante l'invio, il destinatario che riceve correttamente il frame attende un intervallo **SIFS** e risponde obbligatoriamente con un **ACK** a livello MAC.
4. Se il mittente non riceve l'ACK entro il timeout, assume che sia avvenuta una collisione o perdita, raddoppia la finestra di contesa (**Binary Exponential Backoff**) e ritrasmette.

### Meccanismo Opzionale [[RTS-CTS]] (*Virtual Carrier Sense*)
Per risolvere il problema del terminale nascosto sui frame lunghi:
1. Il mittente invia una breve trama di controllo **RTS** (*Request to Send*) indicando la durata totale della trasmissione.
2. Il destinatario risponde dopo un **SIFS** con un **CTS** (*Clear to Send*), udibile anche dai terminali nascosti al mittente.
3. Tutte le stazioni che ascoltano RTS o CTS aggiornano il proprio timer interno **NAV** (*Network Allocation Vector*), astenendosi dal trasmettere fino al completamento dello scambio dati + ACK.

---

## 4. Formato della Trama IEEE 802.11
A differenza di Ethernet (che usa solo 2 indirizzi MAC), la trama 802.11 può contenere fino a **4 indirizzi MAC** per gestire l'instradamento attraverso gli Access Point e il Distribution System:

| Campo | Dimensione | Descrizione |
| :--- | :---: | :--- |
| **Frame Control** | $2\text{ byte}$ | Versione protocollo, Tipo (Management, Control, Data), Sottotipo, bit **To DS** e **From DS**, flag frammentazione/crittografia. |
| **Duration / ID** | $2\text{ byte}$ | Tempo in microsecondi per cui il canale resta occupato (aggiorna il **NAV** delle altre stazioni). |
| **Address 1 (RA)** | $6\text{ byte}$ | *Receiver Address*: indirizzo MAC del dispositivo radio che riceve immediatamente il frame (es. AP). |
| **Address 2 (TA)** | $6\text{ byte}$ | *Transmitter Address*: indirizzo MAC del dispositivo radio che ha trasmesso il frame sul canale. |
| **Address 3** | $6\text{ byte}$ | Indirizzo MAC dell'host finale (destinazione o sorgente) collegato alla rete cablata oltre l'AP. |
| **Sequence Control** | $2\text{ byte}$ | Numero di sequenza ($12\text{ bit}$) per rilevare duplicati e numero di frammento ($4\text{ bit}$). |
| **Address 4** | $6\text{ byte}$ | Presente **solo** nei ponti wireless AP-to-AP ($\text{To DS} = 1$ e $\text{From DS} = 1$, modalità WDS). |
| **Frame Body (Payload)** | $0 - 2312\text{ byte}$ | Dati incapsulati dal livello superiore (LLC/IP). |
| **FCS** | $4\text{ byte}$ | Codice **CRC-32** per la verifica dell'integrità della trama. |