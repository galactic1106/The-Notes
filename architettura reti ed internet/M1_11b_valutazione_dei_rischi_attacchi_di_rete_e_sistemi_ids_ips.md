---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_m.pdf
argomento: Analisi e valutazione dei rischi, minacce e attacchi di rete, sistemi IDS e IPS
tags:
  - reti
  - sicurezza
  - risk-assessment
  - attacchi-rete
  - ids
  - ips
---

# ⚠️ Valutazione dei Rischi, Attacchi di Rete e IDS/IPS

## 1. Criteri di Valutazione dei Rischi (*Risk Assessment*)
La progettazione di un sistema sicuro richiede un'analisi metodica basata su quattro elementi chiave:
* **Asset (Risorsa):** qualsiasi bene hardware, software, dato o servizio di valore per l'organizzazione che deve essere protetto.
* **Vulnerabilità (*Vulnerability*):** debolezza o difetto nella progettazione, implementazione o configurazione di un sistema che può essere sfruttato.
* **Minaccia (*Threat*):** potenziale evento o agente (umano o ambientale) capace di sfruttare una vulnerabilità causando un danno.
* **Rischio (*Risk*):** probabilità che una determinata minaccia sfrutti una vulnerabilità, combinata con l'impatto economico/operativo del danno conseguente:
  $$\text{Rischio} = \text{Probabilità} \times \text{Impatto}$$

### Strategie di Gestione del Rischio
1. **Mitigazione (Riduzione):** introduzione di contromisure tecniche e organizzative (firewall, patch, cifratura, backup).
2. **Trasferimento:** delega dell'impatto finanziario a terzi (es. polizze assicurative cyber o servizi cloud gestiti).
3. **Accettazione:** assunzione consapevole di un rischio residuo basso quando il costo delle contromisure supera il valore dell'asset.
4. **Evitamento:** dismissione del servizio o dell'attività ad alto rischio.

---

## 2. Principali Categorie di Attacchi di Rete

### Attacchi Passivi (Intercettazione)
Mirano a violare la **riservatezza** senza alterare i dati in transito (difficili da rilevare):
* **Packet Sniffing:** cattura dei frame/pacchetti in transito su un segmento di rete impostando la scheda di rete (**NIC**) in **modalità promiscua** (*promiscuous mode*) per leggere traffico non cifrato (credenziali Telnet, HTTP, FTP).
* **Traffic Analysis:** deduzione di informazioni sensibili osservando frequenza, volume e interlocutori delle comunicazioni anche se cifrate.

### Attacchi Attivi (Alterazione, Falsificazione e Blocco)
Mirano a compromettere **integrità**, **autenticazione** o **disponibilità**:
* **Spoofing (Falsificazione dell'identità):**
  * *IP Spoofing:* invio di pacchetti con indirizzo IP sorgente falsificato.
  * *ARP Spoofing / Poisoning:* invio di risposte ARP false su una LAN per associare il proprio MAC address all'IP del gateway, dirottando il traffico locale.
  * *DNS Spoofing / Cache Poisoning:* alterazione delle risposte DNS per reindirizzare gli utenti verso server malevoli.
* **Man-in-the-Middle (MitM):** l'attaccante si interpone segretamente tra due parti comunicanti intercettando, leggendo ed eventualmente modificando i messaggi in tempo reale.
* **DoS (*Denial of Service*) e DDoS (*Distributed DoS*):** esaurimento delle risorse di rete, CPU o memoria di un server per renderlo inaccessibile agli utenti legittimi:
  * *SYN Flood:* invio massivo di segmenti TCP `SYN` con IP sorgente falsificati senza mai completare il *3-way handshake* (`ACK`), saturando la coda delle connessioni semi-aperte del server.
  * *Amplification / Reflection Attacks (es. Smurf, DNS Amplification):* invio di richieste con IP sorgente della vittima verso broadcast o server aperti che generano risposte molto più grandi verso il bersaglio.
* **Malware e Social Engineering:** virus, worm (autoreplicanti via rete), trojan, ransomware e tecniche di **phishing** volte a carpire credenziali ingannando l'utente.

---

## 3. Sistemi [[IDS]] e [[IPS]]

Per individuare e bloccare attività anomale che attraversano il perimetro o la rete interna si affiancano ai firewall i sistemi di rilevamento e prevenzione delle intrusioni:

| Caratteristica | [[IDS]] (*Intrusion Detection System*) | [[IPS]] (*Intrusion Prevention System*) |
| :--- | :--- | :--- |
| **Posizionamento** | Passivo / *Out-of-band* (riceve copia del traffico tramite *SPAN port* o *TAP* di rete) | Attivo / *In-line* (posto direttamente lungo il percorso del traffico come un firewall) |
| **Azione principale** | **Rileva** l'attacco, genera allarmi (*alert*) e registra i log | **Rileva e blocca attivamente** i pacchetti malevoli in tempo reale (*drop*, reset connessione) |
| **Impatto sulla latenza** | Nessun impatto sul flusso dati | Introduce una minima latenza di elaborazione |

### Classificazione per Ambito e Metodo di Rilevazione
* **NIDS (*Network-based IDS*):** analizza il traffico di rete su interi segmenti LAN/DMZ (es. *Snort*, *Suricata*).
* **HIDS (*Host-based IDS*):** monitora log di sistema, chiamate di sistema, processi e integrità dei file critici su un singolo host (es. *OSSEC*, *AIDE*).
* **Signature-based (a firme):** confronta il traffico con un database di pattern noti di attacchi (bassi falsi positivi, ma inefficace contro attacchi *zero-day* sconosciuti).
* **Anomaly-based (ad anomalie / euristico):** costruisce un profilo statistico del comportamento normale della rete (*baseline*) e segnala deviazioni significative (capace di rilevare *zero-day*, ma soggetto a maggiori falsi positivi).