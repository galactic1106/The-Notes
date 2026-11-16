---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
argomento: Livello Fisico ISO/OSI, mezzi wireless e capacità di canale
tags:
  - reti
  - livello-fisico
  - wireless
  - nyquist
  - shannon
  - 5g
---

# 🔌 Livello Fisico (*Physical Layer*) e Mezzi Trasmissivi

## 1. Funzioni del Livello Fisico (Livello 1 ISO/OSI)
Il **Physical Layer** è il livello più basso della pila ISO/OSI e definisce le regole relative alle **connessioni elettriche, meccaniche, funzionali e procedurali** tra dispositivi fisici:
* Specifica le caratteristiche dei cavi, i connettori e il tipo di segnale elettrico/ottico associato ai vari pin delle interfacce.
* Trasporta i singoli **bit** lungo il mezzo trasmissivo adattando i segnali (analogici o digitali) al canale fisico.

---

## 2. Mezzi Trasmissivi – Connessioni Wireless
Le tecnologie senza fili sfruttano la propagazione di onde elettromagnetiche per coprire diversi ambiti applicativi:

### Reti Locali e Personali
* **[[WiFi]] (Reti domestiche e locali):** un *Wireless Router* collega dispositivi senza fili (laptop, console, stampanti) e cablati (PC tramite cavi Ethernet) verso l'esterno tramite un *Cable Modem* o router di accesso.
* **[[Bluetooth]]:** tecnologia wireless a corto raggio (WPAN) suddivisa in **BR/EDR** (*Basic Rate / Enhanced Data Rate*, usato per audio/cuffie, speaker, navigazione auto) e **LE** (*Low Energy*, usato per periferiche PC, smartwatch, beacon, smart home, sensori medicali e fitness).

### Broadcast Televisivo (TV Analogica e Digitale)
* Ricezione tramite antenne terrestri (**DTT** – Digitale Terrestre) e parabole satellitari (**LNB** a 4 uscite a bande separate).
* Distribuzione interna tramite **Centralino TV** e multiswitch (**MSW** radiale attivo) verso le prese domestiche TV/SAT (decoder SAT e televisori DTT).

### Telefonia Mobile (Evoluzione Generazionale)
| Generazione | Velocità tipica | Caratteristiche principali |
| :---: | :--- | :--- |
| **1G** | $2,4\text{ Kbps}$ | Voce analogica (*Analog Voice*). |
| **2G** | $64\text{ Kbps}$ | Voce digitale e dati semplici (*SMS / Digital Voice*). |
| **3G** | $2.000\text{ Kbps}$ ($2\text{ Mbps}$) | Banda larga mobile (*Mobile Broadband*). |
| **4G (LTE)** | $100.000\text{ Kbps}$ ($100\text{ Mbps}$) | Streaming video HD, connettività ad alta velocità. |
| **5G** | $\ge 1\text{ Gbps}$ | Bassissima latenza, IoT massivo e applicazioni real-time. |

---

## 3. Canali Reali: Teoremi di [[Nyquist]] e [[Shannon]]
I canali trasmissivi reali introducono **attenuazioni** e **ritardi** in funzione della frequenza, oltre al **rumore**.

### Teorema di Nyquist (Canale privo di rumore)
Lega la larghezza di banda $B$ di un canale al massimo *data rate* trasportabile:

$$\text{Massimo Data Rate} = 2B \cdot \log_2(V)$$

* **$B$**: larghezza di banda del segnale (in Hz).
* **$V$**: numero di livelli discreti di tensione utilizzati per codificare il segnale.
* *Limite pratico:* aumentando $V$ cresce teoricamente il data rate, ma i livelli di tensione diventano più ravvicinati e **si riduce l'affidabilità** in presenza di rumore.

### Teorema del Campionamento di Shannon ([[PCM]])
Per digitalizzare un segnale analogico (es. voce) tramite **PCM** (*Pulse Code Modulation*) si eseguono tre fasi: **Campionamento**, **Quantizzazione** e **Codifica**.
* Secondo il **Teorema del Campionamento di Shannon**, un segnale a banda limitata compresa tra $f_1$ e $f_2$ (con $f_2 > f_1$) può essere ricostruito fedelmente prelevando campioni a una frequenza $f_c$ almeno doppia rispetto alla frequenza massima del segnale:
  $$f_c \ge 2 f_2$$