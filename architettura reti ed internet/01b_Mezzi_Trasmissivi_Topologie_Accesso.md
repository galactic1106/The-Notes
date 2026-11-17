---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_a.pdf
argomento: Mezzi trasmissivi, topologie di rete e metodi di accesso al mezzo (CSMA/CD e Token)
tags:
  - reti
  - mezzi-trasmissivi
  - topologie
  - csma-cd
  - token-ring
  - fibra-ottica
---

# 🔌 Mezzi Trasmissivi, Topologie e Accesso al Mezzo

## 1. Dai Mainframe alle Prime Reti Locali
* **Era dei Mainframe (anni '60):** calcolatori centralizzati molto costosi con elaborazione *batch* e terminali remoti collegati punto-punto tramite **modem** (es. architettura gerarchica **IBM 3270**: *Mainframe* $\rightarrow$ *Control Unit locale/remota* $\rightarrow$ terminali).
* **Workstation Diskless e [[RARP]]:** stazioni prive di disco rigido che caricavano il sistema operativo dalla rete inviando in broadcast il proprio **MAC address** per ottenere da un server il proprio **indirizzo IP**.

---

## 2. Mezzi Trasmissivi
Il canale fisico su cui viaggiano i segnali si divide in mezzi guidati (cablati) e non guidati (wireless):

| Mezzo Trasmissivo                        | Caratteristiche Principali                                                                                                                                                                     |
| :--------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Doppino intrecciato (*Twisted Pair*)** | Coppie di fili di rame intrecciati per ridurre le interferenze elettromagnetiche (es. cavi UTP/STP per reti Ethernet LAN).                                                                     |
| **Cavo coassiale**                       | Conduttore centrale in rame schermato da una calza metallica; usato nelle prime reti Ethernet (10Base5/10Base2) e nelle reti TV via cavo.                                                      |
| **[[Fibra Ottica]]**                     | Trasmissione tramite impulsi luminosi (LED o laser) in un nucleo di vetro/silice; altissima larghezza di banda, bassissima attenuazione e totale immunità alle interferenze elettromagnetiche. |
| **Etere (Wireless / Onde radio)**        | Propagazione elettromagnetica nello spazio libero (onde radio, microonde, satelliti, infrarossi); soggetta a interferenze e attenuazione.                                                      |

---

## 3. Topologie di Rete
La topologia definisce la disposizione fisica o logica dei nodi e dei collegamenti:

* **Bus:** tutti i nodi sono collegati a un unico canale condiviso (dorsale) terminato alle estremità. Semplice ed economica, ma un guasto sul bus principale blocca l'intera rete e sono necessarie regole di arbitraggio per evitare collisioni.
* **Stella (*Star*):** tutti i nodi sono collegati punto-punto a un concentratore centrale (**[[Hub]]** o **[[Switch]]**). Facile da gestire e isolare i guasti dei singoli rami, ma dipende dal nodo centrale (*single point of failure*).
* **Anello (*Ring*):** ogni nodo è collegato al successivo formando un circuito chiuso unidirezionale; i dati transitano di stazione in stazione (spesso tramite concentratori **MAU**).
* **Albero (*Tree*):** struttura gerarchica multilivello con un nodo radice; facilmente espandibile, ma vulnerabile ai guasti nei nodi di livello superiore.
* **Maglia (*Mesh*):** i nodi sono interconnessi da percorsi multipli (maglia completa o parziale); garantisce elevata tolleranza ai guasti e ridondanza (tipica delle dorsali geografiche e delle reti wireless mesh).

![[architettura reti ed internet/Excalidraw/Topologie_di_Rete|1000]]

---

## 4. Metodi di Accesso al Mezzo Condiviso

### Accesso Conteso (Casuale): [[CSMA-CD]] (*Carrier Sense Multiple Access with Collision Detection*)
Utilizzato nelle reti **[[Ethernet]]** tradizionali su bus/hub condiviso:
1. **Carrier Sense (ascolto della portante):** prima di trasmettere, la stazione ascolta il canale per verificare se è libero.
2. **Multiple Access:** se il canale è libero, la stazione inizia a trasmettere (più stazioni possono farlo contemporaneamente).
3. **Collision Detection:** mentre trasmette, la stazione continua ad ascoltare il mezzo; se rileva una **collisione** (sovrapposizione di segnali), interrompe immediatamente l'invio, invia un segnale di *jam* e attende un tempo casuale (*backoff* esponenziale binario) prima di ritentare.
* **Caratteristica:** metodo **probabilistico / non deterministico** (sotto carico elevato le collisioni aumentano e le prestazioni degradano).

### Accesso Deterministico: *Token Passing* ([[Token Ring]])
Utilizzato nelle reti ad anello (standard **IEEE 802.5**, sviluppato da IBM):
* **Meccanismo del gettone (*Token*):** sull'anello circola continuamente un piccolo frame speciale chiamato **token** (gettone). Solo la stazione che cattura il token libero ha il diritto di trasmettere dati, eliminando alla radice le collisioni.
* **Flusso della trasmissione:** la stazione mittente trattiene il token, immette il frame dati sull'anello; ogni stazione intermedia lo rigenera, il destinatario lo copia e imposta i bit di conferma, e quando il frame torna al mittente questi lo rimuove e rilascia un nuovo token libero.
* **Hardware tipico:** concentratori **MAU** (*Multi-Station Access Unit*, che realizzano un anello logico su cablaggio fisico a stella) e connettori **IBM Type 1** o **RJ-45**.
* **Caratteristica:** metodo **deterministico** (garantisce un tempo massimo di attesa noto anche ad alto carico, ma richiede gestione per perdita o duplicazione del token).

![[Token_Ring_802_5|1000]]

