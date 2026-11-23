---
corso: Architettura Reti ed Internet
modulo: Modulo 2 - Prof. Damiano Perri
università: Università degli Studi di Perugia (UNIPG)
fonte: parte_9.pdf
argomento: Installazione e configurazione dei servizi DHCP, DNS e controllo del traffico tramite ACL
tags:
  - reti
  - dhcp
  - dns
  - acl
  - servizi-rete
  - sicurezza
---

# ⚙️ Servizi Fondamentali di Rete: DHCP, DNS e ACL

## 1. [[DHCP]] (*Dynamic Host Configuration Protocol*)
Protocollo applicativo client/server (RFC 2131, evoluzione di BOOTP) che automatizza l'assegnazione dinamica dei parametri di configurazione TCP/IP agli host di una rete (indirizzo IP, Subnet Mask, Default Gateway, server DNS) per un tempo limitato detto **Lease Time**.

### Trasporto e Porte
* Utilizza **[[UDP]]**: il server resta in ascolto sulla **porta 67**, mentre il client utilizza la **porta 68**.

### Processo DORA (4 Fasi)
1. **`DHCP Discover` (Client $\rightarrow$ Broadcast `255.255.255.255`):** il client privo di indirizzo IP invia una richiesta in broadcast sulla LAN per individuare eventuali server DHCP disponibili.
2. **`DHCP Offer` (Server $\rightarrow$ Client):** uno o più server DHCP rispondono proponendo un indirizzo IP disponibile prelevato dal proprio *pool*, accompagnato da subnet mask e durata del *lease*.
3. **`DHCP Request` (Client $\rightarrow$ Broadcast):** il client seleziona una delle offerte ricevute e notifica in broadcast quale server ha scelto (consentendo agli altri server di liberare gli indirizzi offerti).
4. **`DHCP Ack` (Server $\rightarrow$ Client):** il server selezionato conferma l'assegnazione registrando l'associazione `(MAC Address, IP, Lease)` nel proprio database.

> [!note] DHCP Relay Agent (`ip helper-address`)
> Poiché i messaggi `DHCP Discover` sono invii **broadcast** di Livello 2/3, i router li bloccano per default. Per servire più sottoreti con un unico server DHCP centralizzato si configura sui router interfacciati alle LAN un **DHCP Relay Agent**, che intercetta i broadcast locali e li inoltra in *unicast* al server DHCP remoto.

---

## 2. [[DNS]] (*Domain Name System*)
Sistema distribuito e gerarchico (RFC 1034 / 1035) che traduce i nomi simbolici mnemonici (*FQDN – Fully Qualified Domain Name*, es. `www.unipg.it`) nei rispettivi indirizzi numerici **IPv4 / IPv6** e viceversa.

### Architettura Gerarchica e Risoluzione
* **Struttura ad albero invertito:**
  * **Root Server (`.`):** 13 cluster logici radice distribuiti globalmente tramite *Anycast*.
  * **TLD (*Top-Level Domain*):** domini di primo livello generici (`.com`, `.org`, `.net`) e nazionali/geografici (`.it`, `.uk`, `.de`).
  * **Server Autorevoli (*Authoritative Name Servers*):** detengono i file di zona originali per uno specifico dominio (es. `unipg.it`).
* **Query Ricorsiva vs Iterativa:**
  * Il client invia una **query ricorsiva** al proprio *Local DNS Resolver* (fornito dall'ISP o configurato manualmente), delegandogli l'intero onere della risoluzione.
  * Il Resolver interroga in modo **iterativo** i Root Server, i TLD Server e infine il Server Autorevole, memorizzando il risultato nella propria **cache** per la durata indicata dal parametro **TTL (*Time To Live*)**.
* **Trasporto e Porte:** usa **UDP porta 53** per le interrogazioni standard e **TCP porta 53** per i trasferimenti di zona tra server primario e secondario (*Zone Transfer – AXFR/IXFR*) o risposte superiori a 512 byte.

### Principali Resource Record (RR) DNS
| Tipo Record | Significato e Funzione |
| :--- | :--- |
| **`A` / `AAAA`** | Associa un nome host a un indirizzo **IPv4** (32 bit) o **IPv6** (128 bit). |
| **`NS`** | Indica i *Name Server* autorevoli per la zona DNS. |
| **`CNAME`** | Definisce un alias (*Canonical Name*) che punta a un altro nome host canonico. |
| **`MX`** | Specifica il server di posta elettronica (*Mail Exchanger*) del dominio e la relativa priorità. |
| **`PTR`** | Utilizzato nella risoluzione inversa (*Reverse DNS* nella zona `in-addr.arpa`) per mappare un IP al nome host. |

---

## 3. Controllo del Traffico sui Router: [[ACL]] (*Access Control List*)
Le **ACL** sono elenchi sequenziali di istruzioni (`permit` o `deny`) configurate sulle interfacce di un router per filtrare i pacchetti in ingresso (**`in`**) o in uscita (**`out`**), oppure per selezionare traffico da sottoporre a NAT o QoS.

### Regole di Elaborazione delle ACL
1. **Valutazione Top-Down:** il router confronta l'header di ogni pacchetto con le regole della lista **dall'alto verso il basso** in ordine sequenziale.
2. **First Match:** non appena una regola corrisponde ai criteri del pacchetto, l'azione associata (`permit` o `deny`) viene applicata immediatamente e le regole successive vengono ignorate.
3. **Implicit Deny Any:** al termine di ogni ACL è sempre presente una regola implicita invisibile **`deny any`** che scarta tutto il traffico non esplicitamente autorizzato dalle righe precedenti (pertanto ogni ACL deve contenere almeno una regola `permit`).

### Tipologie di ACL
* **Standard ACL:** filtrano i pacchetti basandosi **esclusivamente sull'indirizzo IP sorgente**; vanno posizionate il più vicino possibile alla **destinazione** per non bloccare traffico legittimo diretto ad altre sottoreti.
* **Extended ACL:** filtrano in base a indirizzo IP sorgente, indirizzo IP destinazione, protocollo (IP, TCP, UDP, ICMP) e numeri di porta sorgente/destinazione; vanno posizionate il più vicino possibile alla **sorgente** per scartare subito il traffico indesiderato.