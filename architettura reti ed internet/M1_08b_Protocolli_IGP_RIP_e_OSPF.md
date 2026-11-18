---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_h.pdf
argomento: Protocolli Interior Gateway Protocol (IGP), RIPv1/RIPv2 e OSPF
tags:
  - reti
  - igp
  - rip
  - ospf
  - routing
  - lsa
---

# 🏘️ Protocolli Interior Gateway Protocol: RIP e OSPF

## 1. [[RIP]] (*Routing Information Protocol*)
È uno dei più antichi protocolli **IGP** di tipo **[[Distance Vector]]**, incapsulato su **[[UDP]]** (porta `520`):

* **Metrica (Hop Count):** utilizza esclusivamente il **numero di salti (router)** attraversati per raggiungere la destinazione, ignorando la reale larghezza di banda o la latenza dei collegamenti.
* **Limite di diametro:** la metrica massima valida è **$15\text{ hop}$**; il valore **$16$** rappresenta **infinito** (rete irraggiungibile), limitando RIP a reti di piccole dimensioni per contenere il problema del *count-to-infinity*.
* **Timer principali:**
  * *Update Timer:* invia l'intera tabella di routing ai vicini ogni **$30\text{ secondi}$**.
  * *Invalid Timer:* se una rotta non viene confermata per **$180\text{ secondi}$**, viene marcata come non valida (metrica $16$).
  * *Flush Timer:* dopo **$240\text{ secondi}$** senza aggiornamenti, la rotta viene eliminata definitivamente dalla tabella.

### Evoluzione: RIPv1 vs RIPv2
* **RIPv1 (RFC 1058):** protocollo *classful* (non trasmette la Subnet Mask negli aggiornamenti, quindi non supporta VLSM né CIDR), invia gli aggiornamenti in **broadcast** (`255.255.255.255`) e non supporta autenticazione.
* **RIPv2 (RFC 2453):** protocollo *classless* (include la Subnet Mask e il Next-Hop), invia gli aggiornamenti in **multicast** (`224.0.0.9`) e supporta l'autenticazione dei messaggi (testo in chiaro o MD5).

---

## 2. [[OSPF]] (*Open Shortest Path First*)
È lo standard IGP aperto (RFC 2328) più diffuso nelle reti aziendali e negli ISP; è un protocollo **[[Link State]]** incapsulato direttamente su **[[IP]]** (`Protocol = 89`):

* **Metrica (Cost):** calcolata in funzione della **larghezza di banda** (*Bandwidth*) delle interfacce lungo il percorso:
  $$\text{Costo} = \frac{\text{Reference Bandwidth}}{\text{Interface Bandwidth}}$$
  *(Con la Reference Bandwidth predefinita a $100\text{ Mbps}$, un link FastEthernet ha costo $1$, un link Ethernet $10\text{ Mbps}$ ha costo $10$).*
* **Caratteristiche operative:** supporto nativo a **CIDR/VLSM**, supporto al bilanciamento del carico su percorsi a costo uguale (**ECMP** – *Equal-Cost Multi-Path*), autenticazione crittografica e aggiornamenti incrementali inviati in multicast (`224.0.0.5` e `224.0.0.6`) solo al variare della topologia.

### Architettura Gerarchica a Aree
Per evitare che il database topologico (**LSDB**) e il calcolo di Dijkstra diventino troppo pesanti su reti molto grandi, OSPF suddivide un Autonomous System in **Aree logiche**:
* **Area 0 (Backbone Area):** area centrale obbligatoria a cui devono essere collegate tutte le altre aree; gestisce il traffico di transito inter-area.
* **Aree Regolari (Standard / Stub):** isolano il *flooding* degli **LSA** (*Link State Advertisement*) dettagliati all'interno della singola area.
* **Tipologie di Router OSPF:**
  * *Internal Router:* tutte le interfacce appartengono alla stessa area.
  * **ABR (*Area Border Router*):** collega una o più aree periferiche all'**Area 0**, riassumendo le rotte tra le aree.
  * **ASBR (*Autonomous System Boundary Router*):** scambia informazioni di routing con reti esterne all'AS (es. verso BGP o rotte statiche).

### Fasi del Funzionamento di OSPF
1. **Hello Protocol:** i router inviano periodicamente pacchetti *Hello* per scoprire i vicini diretti e instaurare relazioni di **adiacenza**.
2. **Elezione di DR e BDR:** sulle reti multi-accesso (es. Ethernet) vengono eletti un **Designated Router (DR)** e un **Backup DR (BDR)** per evitare la formazione di $O(N^2)$ adiacenze complete e centralizzare lo scambio degli LSA.
3. **Sincronizzazione dell'LSDB:** i router adiacenti si scambiano i descrittori del database (*DBD*) e richiedono gli **LSA** mancanti fino ad avere un'immagine topologica identica.
4. **Calcolo dell'albero SPF:** ogni router esegue l'algoritmo di Dijkstra sull'LSDB per popolare la propria tabella di routing.

---

## 3. Confronto Sintetico: RIP vs OSPF

| Caratteristica | RIPv2 | OSPF |
| :--- | :--- | :--- |
| **Algoritmo** | Distance Vector (Bellman-Ford) | Link State (Dijkstra SPF) |
| **Metrica** | Hop Count (max $15$ salti) | Costo basato sulla banda |
| **Incapsulamento** | UDP (porta 520) | Direttamente su IP (Protocol 89) |
| **Convergenza** | Lenta (minuti) | Molto rapida (pochi secondi) |
| **Gerarchia** | Piatta | Gerarchica a due livelli (Area 0 + aree periferiche) |