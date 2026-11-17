---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_h.pdf
argomento: Routing inter-dominio tra Autonomous Systems, Border Gateway Protocol (BGP-4), Path Vector, attributi e politiche
tags:
  - reti
  - bgp
  - egp
  - autonomous-system
  - path-vector
  - peering
---

# 🌍 Routing Inter-Dominio e Protocollo BGP

## 1. Perché Serve un Protocollo Specifico tra AS
All'interno di un Autonomous System (IGP come OSPF), l'obiettivo principale è trovare il percorso **tecnicamente più veloce o più breve** tra due nodi sotto un'unica amministrazione. Tra **Autonomous Systems diversi**, invece, entrano in gioco vincoli **economici, contrattuali e di sicurezza**:

* **Scalabilità globale:** Internet conta oltre $100.000$ AS e circa un milione di prefissi IPv4/IPv6; un protocollo Link State globale sarebbe computazionalmente ingestibile.
* **Policy-Based Routing:** un ISP deve poter decidere quali rotte accettare e quali annunciare in base ad accordi commerciali (es. non trasportare traffico di transito gratuito per concorrenti).
* **Autonomia:** ciascun AS nasconde agli altri la propria topologia interna.

---

## 2. [[BGP]] (*Border Gateway Protocol* – BGP-4)
Il **BGP-4** (RFC 4271) è l'unico protocollo **[[EGP]]** utilizzato per il routing inter-dominio su Internet:

* **Trasporto affidabile:** a differenza di RIP e OSPF, BGP utilizza **[[TCP]] (porta `179`)** per instaurare sessioni persistenti tra coppie di router (**BGP Peers / Speakers**).
* **Aggiornamenti incrementali:** dopo lo scambio iniziale dell'intera tabella di routing, vengono inviati messaggi `UPDATE` **solo** quando una rotta cambia o viene revocata (`WITHDRAWN`), oltre a messaggi periodici `KEEPALIVE` per verificare che la sessione sia attiva.

### Sessioni eBGP vs iBGP
* **eBGP (*External BGP*):** sessione stabilita tra router di confine appartenenti a **Autonomous Systems diversi** (tipicamente direttamente connessi).
* **iBGP (*Internal BGP*):** sessione stabilita tra router appartenenti allo **stesso Autonomous System** per propagare internamente le rotte apprese dall'esterno. *(Regola fondamentale anti-loop di iBGP: un router non inoltra mai a un altro peer iBGP una rotta appresa via iBGP, richiedendo una maglia completa o meccanismi come Route Reflector).*

---

## 3. Algoritmo [[Path Vector]] e Prevenzione dei Loop
BGP adotta un approccio **Path Vector** (evoluzione del Distance Vector):

* Ogni annuncio di raggiungibilità di un prefisso IP (**NLRI**, *Network Layer Reachability Information*) è accompagnato dalla lista ordinata di tutti gli **Autonomous Systems** attraversati per raggiungerlo (**attributo `AS-PATH`**).
* **Prevenzione immediata dei loop:** quando un router di confine riceve un annuncio eBGP, controlla l'attributo `AS-PATH`; se il **proprio numero di AS (ASN) è già presente** nella lista, scarta immediatamente l'annuncio evitando qualsiasi ciclo chiuso a livello inter-dominio.

---

## 4. Principali Attributi BGP e Selezione delle Rotte
In BGP la scelta del percorso migliore non si basa su una semplice metrica numerica, ma sulla valutazione sequenziale di **Attributi di Percorso (*Path Attributes*)** configurati tramite politiche amministrative:

1. **`LOCAL-PREF` (*Local Preference*):** attributo interno all'AS (non propagato all'esterno) usato per indicare ai router interni quale punto di uscita preferire per raggiungere una certa rete remota (valore più alto = preferito).
2. **`AS-PATH`:** sequenza degli ASN attraversati (a parità di `LOCAL-PREF`, si preferisce il percorso con il minor numero di AS attraversati; gli operatori possono allungarlo artificialmente tramite *AS-Path Prepending* per rendere un link meno appetibile).
3. **`ORIGIN`:** indica come la rotta è entrata in BGP (`IGP`, `EGP` o `Incomplete` da ridistribuzione).
4. **`MED` (*Multi-Exit Discriminator*):** suggerimento inviato a un AS adiacente per indicargli quale punto di ingresso preferire quando esistono più collegamenti paralleli tra i due AS (valore più basso = preferito).
5. **`NEXT-HOP`:** indirizzo IP del router di confine verso cui inoltrare i pacchetti diretti al prefisso annunciato.

---

## 5. Relazioni Commerciali tra AS: Transit vs Peering
Le politiche BGP riflettono direttamente gli accordi economici tra operatori:

* **Customer-Provider (Transit):** un ISP più piccolo (cliente) paga un ISP più grande (provider) per ottenere connettività verso l'intera Internet globale. Il provider annuncia le rotte del cliente a tutto il mondo, mentre il cliente annuncia al provider solo le proprie rotte (e quelle dei propri eventuali clienti), **non** facendo da transito tra due suoi provider.
* **Peering (Scambio paritario):** due ISP di dimensioni comparabili concordano di scambiare gratuitamente il traffico diretto esclusivamente ai rispettivi clienti (direttamente o tramite un **[[IXP]]**), senza offrire transito verso il resto di Internet.