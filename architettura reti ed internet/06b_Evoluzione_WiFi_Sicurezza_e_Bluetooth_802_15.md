---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_f.pdf
argomento: Evoluzione standard IEEE 802.11, tecniche di modulazione, sicurezza Wi-Fi (WEP, WPA2, WPA3) e Bluetooth IEEE 802.15
tags:
  - reti
  - wifi
  - sicurezza-wifi
  - wpa2
  - wpa3
  - bluetooth
  - ieee-802-15
---

# 🔐 Evoluzione Wi-Fi, Sicurezza WLAN e Bluetooth (802.15)

## 1. Evoluzione degli Standard [[IEEE 802.11]]
Nel tempo lo standard 802.11 è stato esteso con diversi emendamenti per incrementare banda, efficienza spettrale e portata:

| Standard | Nome Commerciale | Banda di Frequenza | Modulazione / Tecnica | Velocità Max Teorica |
| :--- | :---: | :---: | :---: | :---: |
| **802.11 (1997)** | Legacy | $2.4\text{ GHz}$ | FHSS / DSSS | $2\text{ Mbps}$ |
| **802.11b** | Wi-Fi 1 | $2.4\text{ GHz}$ | HR-DSSS / CCK | $11\text{ Mbps}$ |
| **802.11a** | Wi-Fi 2 | $5\text{ GHz}$ | **OFDM** | $54\text{ Mbps}$ |
| **802.11g** | Wi-Fi 3 | $2.4\text{ GHz}$ | **OFDM** (retrocompatibile 11b) | $54\text{ Mbps}$ |
| **802.11n** | **Wi-Fi 4** | $2.4 / 5\text{ GHz}$ | OFDM + **MIMO** ($4\times 4$), canali $40\text{ MHz}$ | $600\text{ Mbps}$ |
| **802.11ac** | **Wi-Fi 5** | $5\text{ GHz}$ | OFDM + **MU-MIMO**, 256-QAM, canali fino a $160\text{ MHz}$ | $6.9\text{ Gbps}$ |
| **802.11ax** | **Wi-Fi 6 / 6E** | $2.4 / 5 / 6\text{ GHz}$ | **OFDMA**, MU-MIMO bidirezionale, 1024-QAM, Target Wake Time | $9.6\text{ Gbps}$ |
| **802.11be** | **Wi-Fi 7** | $2.4 / 5 / 6\text{ GHz}$ | **MLO** (*Multi-Link Operation*), canali $320\text{ MHz}$, 4096-QAM | $> 40\text{ Gbps}$ |

> [!tip] Canali non sovrapposti a $2.4\text{ GHz}$
> Nella banda ISM a $2.4\text{ GHz}$ sono definiti $13$ canali (in Europa) spaziati di $5\text{ MHz}$, mentre ogni segnale Wi-Fi occupa circa $20 - 22\text{ MHz}$. Di conseguenza, gli unici **tre canali completamente non sovrapposti** sono i canali **1, 6 e 11**.

---

## 2. Sicurezza nelle Reti Wireless (WLAN)
Poiché le onde radio si propagano liberamente nello spazio aperto, chiunque nel raggio di copertura può intercettare il traffico (*sniffing*) o tentare accessi non autorizzati:

1. **[[WEP]] (*Wired Equivalent Privacy* – 1999):**
   * Utilizza l'algoritmo di cifratura a flusso **RC4** con una chiave statica condivisa e un vettore di inizializzazione (**IV**) a soli **$24\text{ bit}$**.
   * **Completamente insicuro:** la breve lunghezza dell'IV causa collisioni frequenti in reti trafficate, consentendo di violare la chiave in pochi secondi.
2. **[[WPA]] (*Wi-Fi Protected Access* – 2003):**
   * Soluzione transitoria basata sul protocollo **TKIP** (*Temporal Key Integrity Protocol*), che genera chiavi dinamiche per ogni pacchetto pur mantenendo l'hardware RC4 esistente.
3. **[[WPA2]] (IEEE 802.11i – 2004):**
   * Sostituisce RC4/TKIP con il protocollo robusto **CCMP** basato su cifratura a blocchi **AES** (*Advanced Encryption Standard*) a $128\text{ bit}$.
   * Prevede due modalità: **WPA2-Personal (PSK)** con chiave pre-condivisa e *4-Way Handshake*, oppure **WPA2-Enterprise** con autenticazione individuale tramite server **RADIUS** e standard **IEEE 802.1X**.
4. **[[WPA3]] (2018):**
   * Introduce il protocollo **SAE** (*Simultaneous Authentication of Equals*, scambio chiavi *Dragonfly*) al posto del PSK, proteggendo contro gli attacchi a dizionario offline e garantendo la **Forward Secrecy**.
   * Supporta la crittografia opportunistica sulle reti aperte (**OWE** – *Opportunistic Wireless Encryption*) e l'obbligo dei **Protected Management Frames (PMF)** contro gli attacchi di deautenticazione.

---

## 3. Reti WPAN e [[Bluetooth]] (Standard IEEE 802.15)
Le reti **[[WPAN]]** (*Wireless Personal Area Network*) coprono distanze ridotte (tipicamente entro $10\text{ m}$) per connettere periferiche personali a basso consumo energetico:

* **Frequenza e Tecnica:** opera nella banda libera ISM a **$2.4\text{ GHz}$** utilizzando la tecnica **FHSS** (*Frequency Hopping Spread Spectrum*): il segnale salta pseudo-casualmente tra $79$ canali da $1\text{ MHz}$ alla frequenza di **$1600\text{ salti/secondo}$** per evitare interferenze con il Wi-Fi.
* **Topologia a Piconet e Scatternet:**
  * **Piconet:** unità base formata da **1 dispositivo Master** (che detta il clock e la sequenza di *frequency hopping*) e fino a **7 dispositivi Slave** attivi contemporaneamente (più eventuali nodi in stato *Parked*). La comunicazione avviene esclusivamente tra Master e Slave tramite **TDD** (*Time Division Duplex*).
  * **Scatternet:** interconnessione di più Piconet adiacenti tramite dispositivi ponte (*bridge node*) che partecipano a più piconet a divisione di tempo.
* **[[BLE]] (*Bluetooth Low Energy* / Bluetooth 4.0+):** variante progettata per dispositivi IoT, sensori e indossabili, ottimizzata per consumi energetici minimi con tempi di connessione nell'ordine dei millisecondi su $40$ canali da $2\text{ MHz}$.