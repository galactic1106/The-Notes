---
corso: Architettura Reti ed Internet
modulo: Modulo 2 - Prof. Damiano Perri
università: Università degli Studi di Perugia (UNIPG)
fonte: parte_7.pdf
argomento: Limiti di velocità nel trasferimento, Decibel, Modem, Interfacce seriali e Multiplazione
tags:
  - reti
  - throughput
  - latenza
  - jitter
  - decibel
  - modem
  - rs232
  - multiplazione
---

# ⚡ Limiti di Velocità, Decibel, Modem, Interfacce e Multiplazione

## 1. Limiti di Velocità nel Trasferimento Dati
Nella valutazione delle prestazioni reali di una rete si distinguono tre parametri fondamentali rispetto alla **velocità trasmissiva** (massima capacità teorica nominale del collegamento, espressa in $\text{bit/s}$):

1. **Throughput:** misura quanto velocemente si riescono effettivamente a spedire i dati (in $\text{bit/s}$) attraverso una determinata rete reale, tenendo conto di sovraccarichi protocollari (*overhead*), ritrasmissioni e traffico concorrente ($\text{Throughput} \le \text{Velocità trasmissiva}$).
2. **Latenza (*Latency* / Ritardo):** tempo totale necessario affinché un messaggio o un pacchetto viaggi dalla sorgente alla destinazione. È la somma di quattro contributi:
   $$\text{Latenza} = T_{\text{propagazione}} + T_{\text{trasmissione}} + T_{\text{accodamento}} + T_{\text{elaborazione}}$$
   * *$T_{\text{propagazione}} = d / v$:* tempo impiegato dal segnale fisico per percorrere la distanza $d$ alla velocità $v$ nel mezzo.
   * *$T_{\text{trasmissione}} = L / R$:* tempo necessario per immettere sul canale un pacchetto di lunghezza $L$ bit alla velocità trasmissiva $R$ ($\text{bit/s}$).
   * *$T_{\text{accodamento}}$ (*queuing delay*):* tempo di attesa nei buffer dei router/switch in caso di congestione.
   * *$T_{\text{elaborazione}}$ (*processing delay*):* tempo richiesto dai nodi intermedi per ispezionare l'header, verificare gli errori e decidere l'instradamento.
3. **Jitter:** variazione statistica della latenza nel tempo tra pacchetti consecutivi dello stesso flusso. Un jitter elevato degrada fortemente le applicazioni multimediali in tempo reale (VoIP, videoconferenze, streaming live), richiedendo l'uso di *jitter buffer* in ricezione.

---

## 2. Il [[Decibel]] ($\text{dB}$) nelle Telecomunicazioni
Il **Bel** è definito come il logaritmo in base $10$ del rapporto tra una grandezza fisica $X$ e un valore di riferimento $X_0$. Poiché il Bel è un'unità molto grande, si utilizza il **Decibel ($\text{dB}$)**, pari a $1/10$ di Bel:

$$G_{\text{dB}} = 10 \log_{10}\left(\frac{P_{\text{out}}}{P_{\text{in}}}\right) \qquad \text{(rapporto di potenze)}$$

* **Proprietà fondamentali:**
  * Se $P_{\text{out}} = P_{\text{in}}$ (rapporto $= 1$) $\implies 0\text{ dB}$ (nessun guadagno né perdita).
  * Se $P_{\text{out}} > P_{\text{in}}$ (rapporto $> 1$) $\implies \text{dB} > 0$ (**amplificazione / guadagno**; es. raddoppio della potenza $\approx +3\text{ dB}$, fattore $10 = +10\text{ dB}$).
  * Se $P_{\text{out}} < P_{\text{in}}$ (rapporto $< 1$) $\implies \text{dB} < 0$ (**attenuazione / perdita**; es. dimezzamento della potenza $\approx -3\text{ dB}$, fattore $1/10 = -10\text{ dB}$).
* **Vantaggio operativo:** permette di trasformare prodotti e quozienti di guadagni/attenuazioni di stadi in cascata in semplici **somme e sottrazioni algebriche**.

---

## 3. [[Modem]] e Interfacce di Comunicazione

### Il Modem (*MOdulator / DEModulator*)
Dispositivo **[[DCE]]** che adatta i segnali digitali prodotti da un terminale **[[DTE]]** (PC/router) per trasmetterli su linee di comunicazione analogiche (come il doppino telefonico **PSTN** o i cavi coassiali CATV) e viceversa:
* **Modulazione (trasmissione):** converte i flussi di bit in variazioni di ampiezza, frequenza o fase di una portante analogica (FSK, PSK, QAM).
* **Demodulazione (ricezione):** estrae l'informazione digitale dal segnale analogico ricevuto dalla linea.

### L'Interfaccia Seriale [[RS-232]] e Controllo di Flusso Locale
Standard EIA/TIA per il collegamento fisico e procedurale tra **DTE** e **DCE** (connettori DB-25 o DE-9):
* **Segnali principali:** `TXD` (trasmissione dati), `RXD` (ricezione dati), `GND` (massa comune), `DTR`/`DSR` (stato operativo degli apparati).
* **Controllo di flusso hardware (`RTS` / `CTS`):** il DTE attiva **RTS** (*Request To Send*) quando ha dati da inviare; il DCE risponde attivando **CTS** (*Clear To Send*) quando è pronto a riceverli.
* **Controllo di flusso software (`XON` / `XOFF`):** utilizza caratteri ASCII di controllo in-band (`XOFF` / DC3 per sospendere l'invio quando il buffer si riempie; `XON` / DC1 per riprendere la trasmissione).

---

## 4. Tecniche di [[Multiplazione]] (*Multiplexing*)
La **multiplazione** consente di condividere un unico mezzo trasmissivo fisico ad alta capacità per trasportare contemporaneamente più flussi comunicativi indipendenti:

1. **[[FDM]] (*Frequency Division Multiplexing*):**
   * La banda passante totale del canale viene suddivisa in più sotto-bande di frequenza disgiunte, separate da **bande di guardia** (*guard bands*) per evitare interferenze.
   * Ogni segnale viene traslato (*shiftato*) su una propria frequenza portante e trasmesso **simultaneamente** agli altri (es. radio FM, TV via cavo, ADSL).
2. **[[WDM]] (*Wavelength Division Multiplexing*):**
   * Analogo ottico della FDM impiegato nelle **fibre ottiche**: più fasci laser a diverse **lunghezze d'onda** ($\lambda$, colori differenti) viaggiano contemporaneamente nella stessa fibra (varianti **CWDM** e **DWDM** – *Dense WDM*).
3. **[[TDM]] (*Time Division Multiplexing*):**
   * L'intera banda del canale è assegnata a turno a ciascuna sorgente per brevi intervalli temporali fissi (**time slot**) organizzati in trame cicliche (es. gerarchia telefonica digitale **PCM / E1**).
   * *Statistical TDM (STDM):* assegna gli slot dinamicamente solo alle sorgenti effettivamente attive, evitando sprechi di banda.