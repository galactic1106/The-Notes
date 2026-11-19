---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_g.pdf
argomento: Protocolli ausiliari del livello Network (ARP, RARP, ICMP) e architettura del protocollo IPv6
tags:
  - reti
  - arp
  - icmp
  - ipv6
  - ping
  - traceroute
---

# 🛠️ Protocolli Ausiliari (ARP, ICMP) e IPv6

## 1. [[ARP]] (*Address Resolution Protocol*)
A livello Data Link i frame vengono consegnati utilizzando l'indirizzo fisico **[[MAC Address]]** ($48\text{ bit}$), mentre il livello Network opera con l'**[[Indirizzo IP]]** ($32\text{ bit}$). Il protocollo **ARP** risolve dinamicamente un indirizzo IPv4 nel corrispondente indirizzo MAC all'interno della **stessa rete locale (LAN)**:

1. L'host $A$ vuole inviare un pacchetto all'IP $X$ (che appartiene alla stessa sottorete; se invece è su una rete remota, $A$ risolve l'IP del **Default Gateway**).
2. $A$ controlla prima la propria **ARP Cache** locale. Se non trova corrispondenza, invia in **broadcast** Ethernet (`FF:FF:FF:FF:FF:FF`) un pacchetto **ARP Request**: *"Chi ha l'IP X? Dillo all'IP A (con MAC_A)"*.
3. Tutti gli host della LAN ricevono la richiesta, ma solo l'host proprietario dell'IP $X$ risponde in **unicast** direttamente a $MAC_A$ con un pacchetto **ARP Reply** contenente il proprio $MAC_X$.
4. Entrambi gli host aggiornano la propria *ARP Cache* con un timer di scadenza (tipicamente qualche minuto).

*(Nota storica: il protocollo inverso **[[RARP]]** permetteva a workstation prive di disco di scoprire il proprio IP noto il MAC; oggi è stato completamente sostituito da **[[DHCP]]**).*

---

## 2. [[ICMP]] (*Internet Control Message Protocol*)
Poiché IP è un protocollo *best-effort* privo di meccanismi di segnalazione, **ICMP** (incapsulato direttamente nei datagrammi IP con `Protocol = 1`) viene utilizzato da router e host per scambiare **messaggi di errore e diagnostica**:

### Principali Tipi di Messaggi ICMP
* **Echo Request (Type 8) / Echo Reply (Type 0):** utilizzati dal comando **`ping`** per verificare la raggiungibilità di un host e misurare il tempo di andata e ritorno (**RTT**).
* **Destination Unreachable (Type 3):** indica che il pacchetto non può essere consegnato (rete irraggiungibile, host irraggiungibile, porta chiusa, oppure necessità di frammentazione con bit `DF = 1`).
* **Time Exceeded (Type 11):** inviato da un router quando il campo **TTL** di un datagramma scende a `0` (o quando scade il timer di riassemblaggio dei frammenti). È sfruttato dal comando **`traceroute` / `tracert`** inviando pacchetti con TTL incrementali ($1, 2, 3, \dots$) per scoprire tutti i router lungo il percorso.
* **Redirect (Type 5):** informa un host della presenza di un router migliore (next-hop più diretto) sulla stessa LAN per raggiungere una determinata destinazione.

---

## 3. Protocollo [[IPv6]] (*Internet Protocol version 6*)
Progettato dall'IETF (RFC 2460 / RFC 8200) per risolvere definitivamente l'esaurimento degli indirizzi IPv4 e semplificare l'elaborazione nei router:

### Caratteristiche Principali e Vantaggi rispetto a IPv4
* **Spazio di indirizzamento a $128\text{ bit}$:** offre $2^{128} \approx 3.4 \times 10^{38}$ indirizzi univoci. Scritto in **notazione esadecimale** suddivisa in $8$ gruppi da $16\text{ bit}$ separati da due punti (es. `2001:0db8:85a3:0000:0000:8a2e:0370:7334`, comprimibile omettendo gli zeri iniziali e sostituendo una sequenza contigua di gruppi nulli con `::`).
* **Header fisso e semplificato ($40\text{ byte}$):**
  * Eliminati i campi *IHL*, *Identification*, *Flags*, *Fragment Offset* e *Header Checksum* (il controllo errori è già garantito dai livelli L2 (Datalink) e L4 (Transport), velocizzando l'inoltro nei router).
  * Le opzioni vengono gestite tramite **Extension Headers** concatenati (*Next Header*) elaborati solo quando necessario.
* **Nessuna frammentazione nei router:** in IPv6 i router intermedi **non frammentano mai** i pacchetti; solo la sorgente può frammentare utilizzando la **Path MTU Discovery** (dimensione minima MTU garantita su ogni collegamento IPv6: $1280\text{ byte}$).
* **Autoconfigurazione e sicurezza:** supporto nativo a **SLAAC** (*Stateless Address Autoconfiguration*), sostituzione di ARP con **NDP** (*Neighbor Discovery Protocol* basato su ICMPv6) e supporto nativo a **[[IPsec]]**.

### Tipologie di Indirizzi IPv6
* **Unicast:**
  * *Global Unicast (`2000::/3`):* indirizzi pubblici instradabili globalmente.
  * *Link-Local (`fe80::/10`):* validi solo sul segmento locale (usati per NDP e routing locale).
* **Multicast (`ff00::/8`):** identifica un gruppo di interfacce (sostituisce completamente il **Broadcast**, che in IPv6 **non esiste**).
* **Anycast:** indirizzo assegnato a più interfacce su nodi diversi; il pacchetto viene consegnato al nodo topologicamente **più vicino** secondo la metrica di routing.