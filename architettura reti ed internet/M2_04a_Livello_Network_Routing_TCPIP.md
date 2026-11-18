---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
docente: Prof. Damiano Perri
argomento: Livello 3 ISO/OSI, Routing Statico e Dinamico, Distance-Vector, Link-State, BGP e Architettura Internet
tags:
  - reti
  - routing
  - network-layer
  - bgp
  - ospf
  - rip
---

# 🌐 Livello Network (Livello 3): Routing TCP/IP e Architettura Internet

## 1. Concetti Fondamentali del Routing
Il **routing** (instradamento) opera al **Livello 3 (Network Layer)** del modello ISO/OSI e rappresenta l'azione di scambiare informazioni in una rete da una sorgente a una destinazione attraversando almeno un nodo intermedio (router/gateway).

Il processo di routing coinvolge due attività principali:
1. **Determinazione del percorso ottimale** (attività computazionalmente complessa affidata ai protocolli di routing).
2. **Trasporto e inoltro (*forwarding*)** dei gruppi di informazioni (pacchetti) attraverso la rete.

### Metrica e Next Hop
* **Metrica (*Metric*):** modalità standard di misura (ad esempio il numero di gateway attraversati, o *hop count*) utilizzata da un protocollo di routing per valutare e stabilire quale sia il percorso ottimale.
* **Associazione `destination / next_hop`:** quando riceve un pacchetto, il router consulta la propria tabella di instradamento e determina quale nodo intermedio successivo (**next hop**) rappresenta il miglior passo verso la destinazione finale.

---

## 2. Routing Statico vs. Routing Dinamico

| Caratteristica | Route Statiche | Route Dinamiche |
| :--- | :--- | :--- |
| **Inizializzazione** | Configurate allo *start-up* | Inizializzate allo *start-up* |
| **Aggiornamento** | **Non cambiano mai** (richiedono intervento manuale) | Aggiornate automaticamente tramite **protocolli di propagazione** tra router |
| **Utilizzo tipico** | Tipiche degli **host** (usate raramente nei router) | Tipiche dei **router** (usate raramente negli host) |

---

## 3. Algoritmi di Routing Dinamico

### A. Distance-Vector Routing
* Ogni router mantiene una tabella contenente per ogni rete nota la tripla `(Destination, Distance, Route/Next Hop)` (con distanza `0` per le reti direttamente connesse).
* Periodicamente i router scambiano il proprio vettore delle distanze con i router adiacenti.
* **Regola di aggiornamento:** se un router riceve da un vicino un percorso verso una destinazione con costo inferiore rispetto a quello attuale (tenendo conto del costo del collegamento verso il vicino stesso), aggiorna la propria tabella sostituendo la distanza e il *next hop*.

### B. Link-State Routing
Sviluppato come alternativa al *distance-vector* per superare la lentezza di convergenza e i problemi derivanti dalla propagazione di informazioni errate:
* **Visione globale (Grafo):** i router sono modellati come nodi di un grafo interconnessi da reti; ogni nodo apprende l'intera topologia della rete.
* **Verifica e Broadcast:**
  1. Periodicamente, le coppie di router direttamente connessi verificano che il link tra loro sia attivo.
  2. Lo stato dei link viene propagato in **broadcast** (*flooding*) a tutta la rete.
* **Calcolo distribuito:** tutti i router ricevono i *routing updates* e ciascuno calcola autonomamente e localmente il cammino ottimale verso ogni destinazione (es. tramite algoritmo di Dijkstra).

---

## 4. Routing Inter-Dominio (AS e BGP) e Infrastruttura Internet

### Autonomous Systems (AS) e Protocolli IGP / EGP
Internet è suddivisa in **Autonomous Systems (AS)**, ovvero reti o insiemi di reti sotto un'unica amministrazione tecnica (identificati da un numero AS, in notazione *asplain*, es. `AS 1274`, `AS 6664`, `AS 9209`).
* **IGP (*Interior Gateway Protocol*):** protocolli usati **all'interno** di un singolo AS (es. **RIP**, **OSPF**).
* **BGP (*Border Gateway Protocol*):** protocollo di routing esterno utilizzato per scambiare informazioni di raggiungibilità tra AS diversi (**eBGP**) e per propagare tali rotte internamente ai router di bordo dello stesso AS (**iBGP**).
* **Connessione Multi-homed:** un AS (o rete utente) collegato contemporaneamente a **due o più provider/AS** distinti (es. tramite sessioni BGP separate) per garantire ridondanza, bilanciamento del carico e tolleranza ai guasti.

### Gerarchia dell'Infrastruttura Internet
L'architettura globale di Internet è strutturata su più livelli gerarchici di ISP (*Internet Service Provider*):
1. **Tier 1 Networks:** dorsali principali di Internet interconnesse globalmente tramite accordi di *peering* gratuito (senza acquistare transito da altri).
2. **Tier 2 Networks / ISP:** reti regionali o nazionali articolate su più **PoP (*Point of Presence*)**; acquistano transito dai Tier 1 e instaurano accordi di *peering* tra loro spesso presso punti di interscambio neutri chiamati **IXP (*Internet Exchange Point*)**.
3. **Tier 3 Networks:** ISP locali che forniscono accesso diretto agli utenti finali (*business* e *consumers*). Possono essere:
   * **Single-homed ISP:** collegati a un solo provider di livello superiore.
   * **Multi-homed ISP:** collegati a più provider di livello superiore per maggiore affidabilità.