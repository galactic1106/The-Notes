---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_m.pdf
argomento: Comandi e strumenti di diagnostica, analisi del traffico e monitoraggio delle reti TCP/IP
tags:
  - reti
  - diagnostica
  - monitoraggio
  - icmp
  - wireshark
  - tcpdump
  - nmap
---

# 🛠️ Strumenti di Diagnostica e Monitoraggio di Rete

## 1. Verifica della Connettività e del Percorso

### `ping` (*Packet Internet Groper*)
* **Funzionamento:** verifica la raggiungibilità a livello IP di un host remoto e misura il tempo di andata e ritorno (**RTT – *Round-Trip Time***) e l'eventuale perdita di pacchetti (*packet loss*).
* **Protocollo:** invia messaggi **[[ICMP]] `Echo Request`** (Type 8) e attende risposte **`Echo Reply`** (Type 0).

### `traceroute` (Unix/Linux) / `tracert` (Windows)
* **Funzionamento:** ricostruisce la sequenza completa dei router (*hop*) attraversati dai pacchetti per raggiungere la destinazione, misurando la latenza di ciascun tratto.
* **Meccanismo tecnico:** sfrutta il campo **TTL (*Time To Live*)** dell'header IPv4 inviando pacchetti probe con $\text{TTL} = 1, 2, 3, \dots$ Ogni router lungo il percorso decrementa il TTL di 1; quando $\text{TTL} = 0$, il router scarta il pacchetto e restituisce al mittente un messaggio **`ICMP Time Exceeded`** (Type 11), rivelando il proprio indirizzo IP. Al raggiungimento dell'host finale viene restituito un `ICMP Port Unreachable` (UDP probe) o `Echo Reply` (ICMP probe).

---

## 2. Configurazione Interfacce, ARP, Routing e Socket

| Comando (Linux / Windows) | Funzione Principale |
| :--- | :--- |
| `ip addr` / `ifconfig` / `ipconfig` | Visualizza e configura indirizzi IP, netmask, indirizzi MAC e stato delle interfacce di rete (`UP`/`DOWN`). |
| `ip route` / `route` | Mostra e modifica la **tabella di instradamento** (*routing table*) locale e il *default gateway*. |
| `ip neigh` / `arp -a` | Consulta e gestisce la **ARP Cache** locale (associazioni dinamiche/statiche tra indirizzi IP e MAC). |
| `ss` / `netstat` | Visualizza le connessioni TCP/UDP attive, le porte in ascolto (`LISTEN`), i processi associati e le statistiche dei protocolli. |
| `dig` / `nslookup` / `host` | Interroga i server **[[DNS]]** per verificare la risoluzione diretta (`A`/`AAAA`), inversa (`PTR`), record `MX` e `NS`. |

---

## 3. Analisi del Traffico (*Packet Sniffing*) e Auditing

### `tcpdump` e [[Wireshark]]
Strumenti di **cattura e analisi dei pacchetti** (*packet analyzer*) basati sulle librerie `libpcap` (Linux/macOS) o `Npcap` (Windows):
* **Modalità di cattura:** pongono l'interfaccia di rete in **modalità promiscua** per catturare tutti i frame in transito sul segmento (o sull'interfaccia selezionata) e salvarli nel formato standard `.pcap` / `.pcapng`.
* **`tcpdump`:** strumento da riga di comando leggero e potente per catture su server remoti tramite SSH con filtri BPF (*Berkeley Packet Filter*, es. `tcpdump -i eth0 port 80 and host 10.0.0.5`).
* **[[Wireshark]]:** analizzatore grafico che decodifica centinaia di protocolli livello per livello (Ethernet $\rightarrow$ IP $\rightarrow$ TCP/UDP $\rightarrow$ HTTP/DNS/TLS), permettendo di ricostruire interi flussi TCP (*Follow TCP Stream*) e individuare ritrasmissioni, colli di bottiglia o anomalie di sicurezza.

### `nmap` (*Network Mapper*)
Strumento standard per l'**esplorazione della rete** e l'**auditing di sicurezza**:
* **Host Discovery:** individua gli host attivi in una sottorete (*ping sweep*).
* **Port Scanning:** verifica quali porte TCP e UDP risultano aperte (`open`), chiuse (`closed`) o filtrate da un firewall (`filtered`), utilizzando tecniche come *TCP Connect Scan* (`-sT`) o *TCP SYN Half-Open Scan* (`-sS`).
* **Service & OS Detection:** identifica versione dei demoni in ascolto (`-sV`) e sistema operativo dell'host target (`-O`).