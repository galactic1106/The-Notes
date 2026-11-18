---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_c.pdf
argomento: Tipologie di canali trasmissivi, teorema di Nyquist e teorema di Shannon
tags:
  - reti
  - nyquist
  - shannon
  - banda
  - capacita-canale
  - rumore
---

# 📐 Limiti Fisici della Trasmissione: Teoremi di Nyquist e Shannon

## 1. Caratteristiche dei Canali Trasmissivi
Nella teoria delle comunicazioni, i canali vengono classificati in base al comportamento rispetto alle frequenze del segnale:
* **Canale perfetto:** trasmette il segnale senza alcuna attenuazione, distorsione o ritardo di propagazione.
* **Canale ideale:** introduce esclusivamente un ritardo di propagazione costante su tutte le frequenze, senza distorcere la forma d'onda.
* **Canale reale:** presenta attenuazione e ritardi variabili in funzione della frequenza, limitazione di banda e **rumore** (*noise*), oltre a fenomeni di interferenza intersimbolica (**[[ISI]]**).

---

## 2. Teorema di [[Nyquist]] (Canale Ideale privo di rumore)
Il teorema di Nyquist mette in relazione la **larghezza di banda $B$** (espressa in $\text{Hz}$) di un canale privo di rumore con la massima velocità di trasmissione teorica (*Maximum Data Rate*):

$$C_{\text{Nyquist}} = 2B \cdot \log_2(V) \quad [\text{bit/s}]$$

* **$B$**: larghezza di banda del canale in $\text{Hz}$ (la massima frequenza di segnalazione è $2B$ simboli/sec, detti **baud**).
* **$V$**: numero di livelli discreti di tensione (o simboli) distinguibili nel segnale.

> **Limite pratico:** aumentando il numero di livelli $V$ si aumenta teoricamente il *data rate*, ma si riduce la distanza tra un livello e l'altro; nei canali reali, in presenza di **rumore**, livelli troppo ravvicinati diventano indistinguibili e causano errori di decodifica.

---

## 3. Teorema di [[Shannon]] (Canale Reale con rumore)
Claude Shannon estese l'analisi ai **canali reali affetti da rumore termico/gaussiano**, determinando il limite superiore assoluto alla capacità di trasmissione priva di errori:

$$C = B \cdot \log_2\left(1 + \frac{S}{N}\right) \quad [\text{bit/s}]$$

* **$C$**: capacità massima del canale in $\text{bit/s}$.
* **$B$**: larghezza di banda del canale in $\text{Hz}$.
* **$S/N$** (*Signal-to-Noise Ratio* / **SNR**): rapporto lineare tra la potenza media del segnale ($S$) e la potenza media del rumore ($N$).

### Conversione del Rapporto Segnale/Rumore in Decibel ($\text{dB}$)
In telecomunicazioni l'SNR è spesso espresso in decibel ($\text{SNR}_{\text{dB}}$):

$$\text{SNR}_{\text{dB}} = 10 \cdot \log_{10}\left(\frac{S}{N}\right) \quad \Longleftrightarrow \quad \frac{S}{N} = 10^{\frac{\text{SNR}_{\text{dB}}}{10}}$$

* **Significato fisico:** anche aumentando arbitrariamente il numero di livelli $V$ nella codifica, non è mai possibile superare la capacità $C$ fissata da banda $B$ e rapporto segnale/rumore $S/N$.