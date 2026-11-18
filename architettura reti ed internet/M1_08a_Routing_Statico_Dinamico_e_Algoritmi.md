---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_h.pdf
argomento: Instradamento a Livello 3, Tabelle di Routing, Routing Statico vs Dinamico, Autonomous Systems, Distance Vector e Link State
tags:
  - reti
  - routing
  - distance-vector
  - link-state
  - autonomous-system
  - dijkstra
---

# 🧭 Fondamenti di Routing, Algoritmi e Autonomous Systems

## 1. Tabelle di Routing e *Longest Prefix Match*
Ogni **[[Router]]** mantiene una **Routing Table** che associa le reti di destinazione all'interfaccia di uscita e all'indirizzo IP del prossimo router lungo il percorso (**Next-Hop**):

* **Voci principali della tabella:** Indirizzo di rete di destinazione (*Network ID*), *Subnet Mask / Prefisso*, *Next-Hop IP*, *Interfaccia di uscita* e *Metrica* (costo del percorso).
* **Regola del *Longest Prefix Match*:** quando un pacchetto arriva al router, l'indirizzo IP di destinazione viene confrontato con tutte le righe della tabella; in caso di corrispondenze multiple, viene selezionata la rotta con il **prefisso di rete più specifico** (ossia la maschera con il maggior numero di bit a `1`, es. `/26` prevale su `/24`).
* **Default Route (`0.0.0.0/0`):** rotta di ultima istanza (*Gateway of Last Resort*) utilizzata quando nessun'altra voce specifica corrisponde alla destinazione.

---

## 2. Routing Statico vs Routing Dinamico

| Caratteristica | Routing Statico | Routing Dinamico |
| :--- | :--- | :--- |
| **Configurazione** | Inserita manualmente dall'amministratore di rete. | Appresa e aggiornata automaticamente tramite protocolli di routing. |
| **Adattabilità ai guasti** | Nessuna (richiede intervento manuale se cade un link). | Elevata: ricalcola automaticamente i percorsi alternativi (*riconvergenza*). |
| **Overhead di rete e CPU** | Nullo (nessun pacchetto di controllo scambiato). | Consumo di banda e risorse CPU/RAM sui router. |
| **Scalabilità** | Adatto solo a reti piccole o *stub network* (con un solo punto di uscita). | Indispensabile per reti medio-grandi e dorsali geografiche. |

---

## 3. Gerarchia di Internet: [[Autonomous System]] (AS)
Poiché Internet è troppo vasta per essere gestita da un'unica tabella di routing piatta, la rete è suddivisa in **Autonomous Systems (AS)**:

* Un **Autonomous System** è un insieme di reti IP e router sotto il controllo di un'unica autorità amministrativa (es. un ISP, un'università, una grande azienda) con una politica di instradamento coerente, identificato globalmente da un **ASN** (*Autonomous System Number* a $16$ o $32\text{ bit}$).
* **[[IGP]] (*Interior Gateway Protocol*):** protocolli di routing utilizzati **all'interno** di un singolo AS (es. **[[RIP]]**, **[[OSPF]]**, IS-IS).
* **[[EGP]] (*Exterior Gateway Protocol*):** protocolli utilizzati per scambiare rotte **tra** Autonomous Systems diversi (lo standard attuale è **[[BGP]]**).

---

## 4. Classi di Algoritmi di Routing Dinamico

### A. Algoritmi [[Link State]] (Stato del Collegamento)
Basati sull'algoritmo di cammino minimo di **Dijkstra (SPF – *Shortest Path First*)**:
* **Visione globale:** ogni router scopre i propri vicini diretti e i costi dei rispettivi collegamenti, poi diffonde queste informazioni a **tutti** i router dell'area tramite pacchetti **LSA** (*Link State Advertisement*) in *flooding*.
* **LSDB (*Link State Database*):** tutti i router costruiscono una mappa topologica completa e identica dell'intera rete.
* **Calcolo locale:** ciascun router esegue indipendentemente l'algoritmo di **Dijkstra** ponendo se stesso come radice per calcolare l'albero dei cammini minimi verso ogni destinazione.
* **Pro e Contro:** convergenza molto rapida e assenza di loop permanenti; richiede maggiore memoria e potenza di calcolo.

### B. Algoritmi [[Distance Vector]] (Vettore delle Distanze)
Basati sull'algoritmo distribuito di **Bellman-Ford**:
* **Visione locale ("instradamento per diceria"):** ogni router conosce inizialmente solo il costo verso i propri vicini diretti e invia periodicamente l'intero proprio vettore delle distanze (`<Destinazione, Costo>`) **soltanto ai vicini direttamente connessi**.
* **Aggiornamento iterativo:** ricevendo il vettore da un vicino $V$, il router $R$ aggiorna la propria distanza verso una destinazione $D$ tramite la relazione di Bellman-Ford:
  $$d_R(D) = \min_{V} \left\{ c(R, V) + d_V(D) \right\}$$
* **Problema del *Count-to-Infinity* (Conteggio all'infinito):** mentre le buone notizie (nuovi percorsi più brevi) si propagano velocemente, la notizia della caduta di un collegamento si propaga lentamente e può generare **loop di routing temporanei** in cui i router si rimbalzano rotte obsolete incrementando progressivamente la metrica fino a infinito.
* **Contromisure principali:**
  * **Split Horizon:** un router non riannuncia mai una rotta sulla stessa interfaccia da cui l'ha appresa.
  * **Poison Reverse:** una rotta appresa da un vicino viene riannunciata a quel vicino con metrica pari a **infinito** (irraggiungibile).
  * **Triggered Updates:** invio immediato dell'aggiornamento appena si verifica un guasto, senza attendere il timer periodico.