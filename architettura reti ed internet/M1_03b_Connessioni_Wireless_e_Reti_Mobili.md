---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_c.pdf
argomento: Mezzi trasmissivi non guidati, tecnologie wireless (PAN/LAN/MAN/WAN) e generazioni cellulari
tags:
  - reti
  - wireless
  - wifi
  - bluetooth
  - reti-mobili
  - 5g
---

# 📶 Connessioni Wireless e Reti Radiomobili

## 1. Classificazione delle Reti Wireless per Estensione
I mezzi trasmissivi non guidati utilizzano lo spettro elettromagnetico (onde radio, microonde, infrarossi) per collegare dispositivi senza cablaggio fisico:

| Scala / Ambito | Sigla | Tecnologie e Standard Principali |
| :--- | :--- | :--- |
| **Personal Area Network** | **[[WPAN]]** | **[[Bluetooth]]** (IEEE 802.15.1), **UWB** (IEEE 802.15), HomeRF, Infrarossi (IrDA) |
| **Local Area Network** | **[[WLAN]]** | **[[Wi-Fi]]** (famiglia **IEEE 802.11** a/b/g/n/ac/ax) |
| **Metropolitan Area Network** | **[[WMAN]]** | **[[WiMAX]]** (IEEE 802.16), **Wi-Fi Mesh** / *Mesh Networks* |
| **Wide Area Network** | **[[WWAN]]** | Reti radiomobili cellulari (**2G, 3G, 4G/LTE, 5G**), collegamenti satellitari |

---

## 2. Principali Tecnologie Wireless Locale e Metropolitana
* **[[Wi-Fi]] (IEEE 802.11):** consente l'accesso wireless a reti locali (LAN) tramite *Access Point* (AP) operando principalmente nelle bande non licenziate a **$2.4\,\text{GHz}$** e **$5\,\text{GHz}$** (oltre a $6\,\text{GHz}$ nelle generazioni più recenti).
* **[[Bluetooth]] (IEEE 802.15):** tecnologia radio a corto raggio e basso consumo progettata per interconnettere periferiche personali (*piconet* e *scatternet*) nella banda ISM a $2.4\,\text{GHz}$.
* **Broadcast TV Digitale e Satellitare:** distribuzione unidirezionale a larga copertura tramite trasmettitori terrestri (DVB-T/T2) o satelliti geostazionari (DVB-S/S2).

---

## 3. Evoluzione delle Reti Radiomobili Cellulari (1G – 5G)
Le reti mobili suddividono il territorio in **celle**, ciascuna servita da una stazione radio base (*Base Station*), permettendo il riutilizzo delle frequenze e la mobilità continua (*handover*):

1. **1G (Prima Generazione):** sistemi analogici dedicati esclusivamente alla voce (es. TACS, AMPS).
2. **2G (Seconda Generazione):** passaggio alla trasmissione digitale con **[[GSM]]** (*Global System for Mobile Communications*); introduzione degli SMS e primi servizi dati a bassa velocità (**GPRS** - 2.5G, **EDGE** - 2.75G).
3. **3G (Terza Generazione):** standard **[[UMTS]]** con accesso a banda larga mobile, videochiamate e navigazione Internet potenziata da **HSPA** (**HSDPA** in downlink e **HSUPA** in uplink).
4. **4G (Quarta Generazione):** architettura interamente basata su pacchetti IP (**[[LTE]]** – *Long Term Evolution* / LTE-Advanced), con elevate velocità di trasferimento per streaming HD e applicazioni real-time.
5. **5G (Quinta Generazione):** latenza ultrabassa (ordine del millisecondo), velocità multi-gigabit ed elevatissima densità di dispositivi connessi per **[[IoT]]**, automazione industriale e veicoli connessi.