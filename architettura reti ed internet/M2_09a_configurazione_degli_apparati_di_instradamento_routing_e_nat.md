---
corso: Architettura Reti ed Internet
modulo: Modulo 2 - Prof. Damiano Perri
università: Università degli Studi di Perugia (UNIPG)
fonte: parte_9.pdf
argomento: Architettura, installazione e configurazione di Router/Switch, Tabelle di Routing e NAT/PAT
tags:
  - reti
  - routing
  - router
  - switch
  - nat
  - pat
  - configurazione
---

# 🖧 Configurazione degli Apparati di Instradamento, Routing e NAT

## 1. Architettura Interna di un [[Router]]
Un **Router** è un sistema di elaborazione dedicato all'interconnessione di reti eterogenee al **Livello 3 (Network)** del modello ISO/OSI. I suoi componenti hardware/software principali sono:
* **CPU e Memoria RAM:** esegue il sistema operativo di rete (es. Cisco IOS, Linux/FRRouting) e mantiene le strutture dati dinamiche: la **Tabella di Routing**, la **ARP Cache**, i buffer dei pacchetti e la configurazione attiva (*running-config*).
* **NVRAM (*Non-Volatile RAM*):** conserva il file di configurazione di avvio (*startup-config*) anche in assenza di alimentazione.
* **Memoria Flash:** memoria riscrivibile contenente l'immagine compressa del sistema operativo.
* **ROM:** contiene il *bootstrap* per l'avvio iniziale, la diagnostica hardware (**POST** – *Power-On Self-Test*) e un'immagine minimale di ripristino.
* **Interfacce di Rete:** porte LAN (FastEthernet/GigabitEthernet) per le sottoreti locali, porte WAN seriali/ottiche per i collegamenti geografici e porte di gestione (**Console** seriale/USB e **AUX**).

---

## 2. Configurazione Base e Modalità Operative (CLI)
La configurazione degli apparati di instradamento (in particolare in ambienti Cisco IOS) si articola in livelli gerarchici con privilegi crescenti:
1. **User EXEC Mode (`Router>`):** modalità di sola consultazione con comandi diagnostici limitati (`ping`, `traceroute`, `show version`).
2. **Privileged EXEC Mode (`Router#`):** accessibile tramite il comando `enable`; consente di ispezionare l'intera configurazione (`show running-config`, `show ip route`, `show ip interface brief`), salvare le modifiche (`copy running-config startup-config`) e riavviare l'apparato.
3. **Global Configuration Mode (`Router(config)#`):** accessibile tramite `configure terminal`; permette di modificare parametri globali (hostname, password di accesso, abilitazione del routing) e di accedere ai sotto-contesti specifici delle singole interfacce (`interface gigabitEthernet 0/0`) o dei protocolli di routing (`router ospf 1`).

> [!tip] Procedura essenziale di attivazione di un'interfaccia
> Sui router, a differenza degli switch, tutte le interfacce fisiche sono **spente per default** (`administratively down`). Per renderle operative occorre assegnare indirizzo IP e subnet mask (`ip address <ip> <mask>`) ed eseguire esplicitamente il comando **`no shutdown`**.

---

## 3. Gestione del Routing: Statico, Dinamico e Default Route
Il router decide l'inoltro di ciascun datagramma IP consultando la propria **Routing Table** secondo il principio del **Longest Prefix Match** (vince la rotta con la maschera di sottorete più specifica, ovvero con il maggior numero di bit a `1`).

### Tipologie di Rotte
* **Rotte Direttamente Connesse (`C`):** inserite automaticamente nella tabella non appena un'interfaccia viene configurata con un IP e portata nello stato `UP`.
* **Routing Statico (`S`):** rotte configurate manualmente dall'amministratore (`ip route <rete_dest> <mask> <next_hop | interfaccia_uscita>`).
  * *Vantaggi:* zero consumo di banda per aggiornamenti, zero carico sulla CPU, massima prevedibilità e sicurezza.
  * *Limiti:* nessuna riconfigurazione automatica in caso di guasto di un collegamento; scarsa scalabilità in reti grandi.
* **Default Route (`S*` – Rotta di Default `0.0.0.0/0`):** rotta *gateway of last resort* utilizzata quando nessuna voce specifica nella tabella corrisponde all'indirizzo IP di destinazione (tipica sui router perimetrali verso l'ISP).
* **Routing Dinamico (`R`, `O`, `B`):** rotte apprese e aggiornate automaticamente tramite protocolli di routing (**[[RIP]]**, **[[OSPF]]**, **[[BGP]]**), selezionando il percorso migliore in base alla **Distanza Amministrativa (AD)** (affidabilità della fonte di routing) e alla **Metrica** (hop count, banda, costo).

---

## 4. [[NAT]] (*Network Address Translation*) e [[PAT]]
Per fronteggiare l'esaurimento degli indirizzi **IPv4** pubblici e mascherare la topologia interna della LAN (indirizzi privati RFC 1918: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`), i router perimetrali implementano la traduzione degli indirizzi:

1. **Static NAT (1:1):** associa in modo permanente un singolo indirizzo IP privato interno a un singolo indirizzo IP pubblico esterno (usato per server interni raggiungibili dall'esterno).
2. **Dynamic NAT (N:M):** assegna dinamicamente a un host interno un indirizzo IP pubblico libero prelevato da un *pool* di indirizzi pubblici disponibili.
3. **[[PAT]] (*Port Address Translation* / NAT Overload – N:1):** consente a **migliaia di host interni** di condividere contemporaneamente un **unico indirizzo IP pubblico** distinguendo i flussi tramite i **numeri di porta TCP/UDP** sorgente (memorizzati nella *NAT Translation Table* del router).
4. **Port Forwarding (Virtual Server):** inoltra le richieste provenienti da Internet su una specifica porta pubblica (es. TCP 80 o 443) verso l'IP privato e la porta di un server interno alla LAN o alla DMZ.