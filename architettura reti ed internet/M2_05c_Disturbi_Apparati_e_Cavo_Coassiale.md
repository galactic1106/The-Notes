---
corso: Architettura Reti ed Internet
modulo: Modulo 2 - Prof. Damiano Perri
università: Università degli Studi di Perugia (UNIPG)
argomento: Attenuazione, distorsione, modalità duplex, DTE/DCE/CPE e cavo coassiale
tags:
  - reti
  - attenuazione
  - distorsione
  - dte-dce
  - cavo-coassiale
  - ethernet
---

# 🔌 Degradazione del Segnale, Apparati e Cavo Coassiale

## 1. Attenuazione, Rigenerazione e Distorsione
* **Attenuazione:** perdita di potenza del segnale lungo il mezzo trasmissivo[cite: 17].
  * Nei **segnali analogici** si utilizza un **amplificatore**, che però amplifica anche il rumore sovrapposto[cite: 17].
  * Nei **segnali digitali** si effettua la **rigenerazione**, ricostruendo la forma d'onda quadra pulita ed eliminando il rumore accumulato[cite: 17].
* **Distorsione:** alterazione della forma del segnale dovuta al fatto che le diverse componenti in frequenza viaggiano con ritardi differenti[cite: 17]. È un fenomeno intrinseco al sistema (svanisce quando il sistema è spento)[cite: 17]; in ricezione i bit vengono comunque riconosciuti correttamente se ai punti di campionamento il livello supera o resta sotto la **soglia di riconoscimento** (es. $2,5\text{ V}$ per un segnale $0\text{–}5\text{ V}$)[cite: 17].

---

## 2. Modalità di Trasmissione e Apparati di Rete
### Modalità Duplex
* **Half-Duplex:** ciascun dispositivo può sia trasmettere che ricevere, ma **non contemporaneamente** (es. walkie-talkie o ponte a una sola corsia)[cite: 19].
* **Full-Duplex:** entrambi i dispositivi possono trasmettere e ricevere **contemporaneamente** (es. tramite due collegamenti fisici separati)[cite: 19].

### Terminologia degli Apparati ([[DTE]], [[DCE]], [[CPE]])
* **DTE (*Data Terminal Equipment*):** dispositivo informatico terminale in cui risiede l'applicazione utente (es. PC, host)[cite: 19].
* **DCE (*Data Circuit-Terminating Equipment*):** dispositivo che adatta e converte i segnali del DTE per l'invio sul canale di comunicazione (es. modem)[cite: 19].
* **CPE (*Customer Premises Equipment*):** apparato di rete installato presso la sede/abitazione dell'utente per l'accesso ai servizi dell'operatore (es. terminazioni ISDN, wireless o VoIP)[cite: 19].

---

## 3. Mezzi Trasmissivi: Cavo Coassiale
Il **cavo coassiale** è formato da un conduttore centrale in rame, un isolante dielettrico, una calza schermante in rame e una guaina esterna in polietilene[cite: 20]. Si distingue in base all'**impedenza caratteristica**[cite: 20]:
* **$75\ \Omega$:** utilizzato per segnali televisivi analogici e digitali[cite: 20].
* **$50\ \Omega$:** utilizzato per strumentazione di misura e trasmissione dati (reti Ethernet storiche e ArcNet)[cite: 20].

### Varianti Ethernet su Cavo Coassiale ($50\ \Omega$)
| Tipo di cavo | Sigla | Diametro | Connessione e caratteristiche |
| :--- | :--- | :--- | :--- |
| **Thick Ethernet** | **RG-8** | $0,4''$ | Utilizza un **transceiver** collegato alla scheda di rete tramite cavo a 9 fili (lunghezza max $50\text{ m}$) e connettore **AUI a 15 pin**[cite: 20]. |
| **Thin Ethernet** | **RG-58** | $0,25''$ | Utilizza connettori **BNC a T** con innesto a baionetta[cite: 20]. |