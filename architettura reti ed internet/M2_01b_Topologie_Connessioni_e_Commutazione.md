---
corso: Architettura Reti ed Internet
modulo: Modulo II - Prof. Damiano Perri
fonte: parte_1.pdf
argomento: Topologie di rete, DTE/DCE, connessioni point-to-point/multipunto e tecniche di commutazione
tags:
  - reti
  - modulo-2
  - perri
  - topologie
  - commutazione
  - dte-dce
---

# 🕸️ Topologie di Rete, Connessioni e Commutazione

## 1. Terminologia di Base: [[DTE]] e [[DCE]]
* **[[DTE]] (*Data Terminal Equipment*):** dispositivo terminale utente che genera o riceve i dati (es. PC, workstation, server).
* **[[DCE]] (*Data Circuit-terminating Equipment*):** apparato di interfaccia che collega il DTE alla linea di comunicazione occupandosi della conversione/adattamento del segnale (es. modem, CSU/DSU).

---

## 2. Tipi di Connessione (Configurazione di Linea)

### A. Connessione Point-to-Point (Punto-Punto)
* Collegamento diretto ed esclusivo tra **due soli DTE** (es. tramite linea telefonica o linea dedicata).
* L'intera capacità del canale è riservata esclusivamente alla comunicazione tra quei due nodi.

### B. Connessione Multipoint (Multi-punto / Condivisa)
* Collegamento condiviso tra **più DTE** sullo stesso mezzo trasmissivo.
* L'intera capacità del canale viene ripartita tra tutti i dispositivi connessi secondo due modalità:
  * **Condivisione temporale:** l'accesso al mezzo trasmissivo è esclusivo a turni (es. TDM o protocolli a contesa/token).
  * **Condivisione dello spazio (o frequenza):** è ammesso l'uso simultaneo del canale da parte di più DTE (es. FDM).

---

## 3. Topologie Fisiche e Logiche di Rete
La **topologia** definisce il modo in cui i nodi sono interconnessi fisicamente o logicamente:

| Topologia | Struttura | Vantaggi | Svantaggi |
| :--- | :--- | :--- | :--- |
| **Bus** | Tutti i nodi sono collegati a un unico cavo dorsale condiviso (*backbone*) terminato alle estremità. | Semplicità di cablaggio, basso costo iniziale. | Un guasto sul cavo principale blocca l'intera rete; collisioni. |
| **Stella (*Star*)** | Ogni nodo è collegato tramite un link *point-to-point* a un concentratore centrale ([[Hub]] o [[Switch]]). | Facile gestione e isolamento dei guasti sui singoli rami. | *Single point of failure* sul nodo centrale. |
| **Anello (*Ring*)** | Ogni nodo è connesso esattamente a due nodi adiacenti formando un percorso chiuso unidirezionale o bidirezionale. | Accesso ordinato senza collisioni (es. Token Ring). | L'interruzione di un collegamento può bloccare l'anello se non ridondato. |
| **Maglia (*Mesh*)** | I nodi sono interconnessi con collegamenti multipli (*Full Mesh*: ogni nodo collegato a tutti gli altri con $\frac{n(n-1)}{2}$ link; *Partial Mesh*: ridondanza selettiva). | Altissima affidabilità e tolleranza ai guasti; instradamenti alternativi. | Costo elevato e complessità di cablaggio nelle reti cablate. |
| **Albero (*Tree*)** | Struttura gerarchica ottenuta interconnettendo più topologie a stella. | Ottima scalabilità e segmentazione gerarchica. | Dipendenza dai nodi dorsali superiori. |

---

## 4. Reti Dedicate vs Reti Commutate
* **Rete Dedicata (*Leased Line*):** collegamento permanente riservato a due utenti; garantisce banda costante ma ha costi elevati e scarsa flessibilità.
* **Rete Commutata (*Switched Network*):** i nodi intermedi (*switch* / *router*) instradano i dati dinamicamente tra sorgente e destinazione condividendo le risorse trasmissive tra molti utenti.

### Tecniche di Commutazione
1. **Commutazione di Circuito (*Circuit Switching*):**
   * Viene instaurato un percorso fisico/logico dedicato prima della trasmissione (fasi: *setup*, *trasferimento*, *rilascio*), tipico della rete telefonica tradizionale (**PSTN**).
   * Ritardo costante, ma risorse sprecate durante le pause di trasmissione.
2. **Commutazione di Pacchetto (*Packet Switching*):**
   * I messaggi vengono suddivisi in blocchi (**pacchetti**) dotati di intestazione (*header*) con indirizzo di destinazione e instradati indipendentemente nodo per nodo (*Store-and-Forward*).
   * Ottimizza l'utilizzo della banda ed è alla base di **Internet**.