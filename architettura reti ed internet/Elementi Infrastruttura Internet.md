---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
argomento: Elementi dell'infrastruttura di Internet
tags:
  - reti
  - infrastruttura
  - isp
  - networking
  - cloud
---

# 🏗️ Elementi dell'Infrastruttura di Internet

L'architettura dell'infrastruttura di Internet connette gli utenti finali ai contenuti online attraversando una gerarchia di reti, fornitori di accesso (ISP) e dorsali ad alta capacità.

---

## 1. Lato Utente (*User Location*)

### User PC (Terminale Multimediale)
PC o dispositivo mobile equipaggiato per inviare e ricevere flussi audio, video e dati protetti:
* **Audio:** scheda audio, microfono, casse/headset, supporti MIDI e riconoscitori vocali.
* **Video:** display, scheda grafica (GPU), videocamera e webcam.
* **Sicurezza:** moduli di cifratura (*Encryption*).

### User's Communication Equipment (CPE – *Customer Premise Equipment*)
Apparati situati presso l'utente (casa o azienda) per collegare i dispositivi locali al *Local Loop*:
* **Connettività di accesso:** linea telefonica [[ADSL]] (ADSL Forum) o fibra ottica ([[FTTH]]).
* **Apparati di rete:** Modem/Router con funzioni integrate di **[[NAT]]**, **[[DHCP]]**, **[[Firewall]]** e **Port Forwarding**.
* **Rete locale:** infrastruttura cablata ([[Ethernet]] / LAN) o wireless ([[WiFi]]) e server aziendali interni (DNS, Email, sicurezza).

---

## 2. Rete di Accesso: *Local Loop Carrier*
È l'infrastruttura ("ultimo miglio") che connette la sede dell'utente al punto di accesso dell'ISP tramite diverse tecnologie trasmissive:
* **Cavo in rame:** linee telefoniche tradizionali (POTS), DSL / ADSL e linee dedicate (*Leased Lines*).
* **Fibra ottica:** collegamenti FTTH (*Fiber To The Home*).
* **Reti via cavo:** *Cable network* (infrastrutture TV via cavo).
* **Senza fili:** reti *Wireless*, ponti radio e collegamenti satellitari (*Satellite*).
* **Onde convogliate:** *Electric Power Lines* (trasmissione su rete elettrica).

---

## 3. Internet Service Provider (ISP) e Servizi

### ISP's Point of Presence ([[POP]])
* È il punto di accesso fisico all'ISP (generalmente il *data center* centrale).
* Le connessioni vengono veicolate dalle centrali del fornitore dell'ultimo miglio (es. Telecom) fino al fornitore di servizi che vende la connettività all'utente finale.

### User Services (Servizi per l'Utente)
Host e server gestiti per fornire i servizi essenziali durante la navigazione:
* **[[DNS]] (Domain Name Server):** es. *BIND*.
* **Email Host:** gestione della posta elettronica (es. *Sendmail*, *Postfix*).
* **Servizi Speciali e Hosting:** accesso remoto sicuro ([[SSH]]), *User Web Hosting*, Usenet e server applicativi/gaming.
* **Requisiti hardware:** collegamenti ad altissima velocità, CPU potenti, grande quantità di memoria e architetture **fault tolerant** e **load balanced**.

---

## 4. Dorsali e Gerarchia di Internet (*ISP Backbone*)

### ISP Backbone
Il *backbone* (dorsale) interconnette tra loro i vari POP di un ISP e collega l'ISP agli altri operatori e ai fornitori di contenuti:
* **Componenti:** *Backbone Providers*, circuiti ad altissima capacità in fibra ottica (*Large Capacity Circuits*), router ad alte prestazioni, switch **SONET/SDH** e **Gigaswitch**.
* **Interconnessione tra operatori:** avviene tramite accordi diretti (**Private Peering**) oppure presso punti di scambio neutri ad alta capacità chiamati **[[NAP]]** (*Neutral Access Point*) o **[[IXP]]** (*Internet Exchange Point*), progettati per ottimizzare il routing del traffico tra ISP diversi.

### Gerarchia degli ISP (Tier)
L'architettura globale si organizza su tre livelli principali:
1. **Tier 1 Networks:** grandi dorsali globali interconnesse tra loro tramite *peering* senza pagare transito.
2. **Tier 2 Networks / ISP:** operatori regionali o nazionali che collegano i propri POP alle reti Tier 1 e fanno peering presso gli IXP.
3. **Tier 3 Networks:** ISP locali che forniscono accesso diretto agli utenti finali (*business* e *consumers*); possono essere **single-homed** (collegati a un solo provider superiore) o **multi-homed** (collegati a più provider per ridondanza).

---

## 5. Contenuti Online (*Online Content*)
Sono gli host e le piattaforme con cui interagiscono gli utenti finali:
* **Infrastrutture:** *Web Server platforms* e *Hosting Farms* (per contenuti web, audio e video).
* **[[Cloud Computing]]:** offre soluzioni affidabili a costi sostenibili, facilmente configurabili per garantire **scalabilità** e **alta affidabilità**.

> [!info] Collegamenti correlati
> Vedi anche [[Storia di Internet]] (sezione *Origins of Online Content*) e [[Intranet_Extranet_DMZ]] per l'architettura delle reti aziendali.