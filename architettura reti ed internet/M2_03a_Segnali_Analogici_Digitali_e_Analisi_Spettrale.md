---
corso: Architettura Reti ed Internet
modulo: Modulo II - Prof. Damiano Perri
fonte: parte_3.pdf
argomento: Segnali analogici e digitali, analisi di Fourier, Decibel e teoremi di Nyquist e Shannon
tags:
  - reti
  - modulo-2
  - perri
  - segnali
  - fourier
  - decibel
  - nyquist
  - shannon
---

# 📈 Segnali Analogici e Digitali, Fourier e Capacità del Canale

## 1. Funzione dello Strato Fisico e Natura dei Segnali
La funzione principale dello **strato fisico** (*Physical Layer*) è trasportare i dati adattando l'informazione alle caratteristiche fisiche del mezzo trasmissivo. I segnali elettromagnetici o elettrici si dividono in due categorie fondamentali:

* **Segnale Analogico:** grandezza fisica continua nel tempo e nei valori assunti (es. onda sinusoidale $s(t) = A \sin(2\pi f t + \phi)$).
* **Segnale Digitale:** sequenza di impulsi discreti che assumono un numero finito di livelli di ampiezza (tipicamente due livelli binari: `0` e `1`).

### Parametri Fondamentali di un Segnale Periodico
* **Ampiezza ($A$):** valore massimo (di picco) raggiunto dal segnale, misurato in Volt ($\text{V}$).
* **Periodo ($T$):** intervallo di tempo necessario al segnale per compiere un ciclo completo, misurato in secondi ($\text{s}$).
* **Frequenza ($f$):** numero di cicli compiuti nell'unità di tempo, misurata in Hertz ($\text{Hz}$):
  $$f = \frac{1}{T}$$
* **Fase ($\phi$):** spostamento angolare del segnale rispetto all'origine dei tempi ($t = 0$), misurato in radianti o gradi.
* **Lunghezza d'onda ($\lambda$):** distanza fisica percorsa dal segnale durante un intero periodo, legata alla velocità di propagazione $v$ nel mezzo:
  $$\lambda = v \cdot T = \frac{v}{f}$$

---

## 2. Analisi di [[Fourier]] e Larghezza di Banda
Secondo il **Teorema di Fourier**, qualsiasi segnale periodico $g(t)$ di periodo $T$ (inclusa un'onda quadra digitale) può essere scomposto come somma infinita di funzioni sinusoidali (**armoniche**) aventi frequenze multiple della frequenza fondamentale $f_0 = \frac{1}{T}$:

$$g(t) = \frac{1}{2} c + \sum_{n=1}^{\infty} a_n \sin(2\pi n f_0 t) + \sum_{n=1}^{\infty} b_n \cos(2\pi n f_0 t)$$

* **Spettro del segnale:** insieme delle frequenze delle componenti armoniche che costituiscono il segnale.
* **Larghezza di banda del segnale ($B$):** intervallo di frequenze in cui è concentrata la maggior parte dell'energia del segnale ($B = f_{\max} - f_{\min}$).
* **Banda passante del canale:** intervallo di frequenze che il mezzo trasmissivo lascia passare senza eccessiva attenuazione. Se la banda del canale è troppo stretta rispetto alla velocità di trasmissione, le armoniche superiori vengono tagliate e il segnale digitale subisce **distorsione** (*arrotondamento degli impulsi*).

---

## 3. Disturbi Trasmissivi e [[Decibel]] ($\text{dB}$)
Durante la propagazione lungo il canale, il segnale è soggetto a tre fenomeni degradanti:
1. **Attenuazione:** perdita progressiva di potenza/ampiezza del segnale all'aumentare della distanza e della frequenza.
2. **Distorsione:** alterazione della forma d'onda dovuta alla diversa velocità di propagazione delle varie armoniche nel mezzo.
3. **Rumore (*Noise*):** segnali indesiderati sovrapposti al segnale utile (rumore termico, diafonia/*crosstalk*, rumore impulsivo).

### Calcolo in Decibel ($\text{dB}$)
Il **decibel** è un'unità logaritmica adimensionale utilizzata per esprimere il rapporto tra due potenze ($P_1, P_2$) o due tensioni ($V_1, V_2$), utile per trasformare moltiplicazioni di guadagni/attenuazioni in semplici somme:

$$\text{Guadagno/Attenuazione (dB)} = 10 \log_{10}\left(\frac{P_{\text{out}}}{P_{\text{in}}}\right) = 20 \log_{10}\left(\frac{V_{\text{out}}}{V_{\text{in}}}\right)$$

* Se $P_{\text{out}} < P_{\text{in}}$, il valore in $\text{dB}$ è **negativo** (attenuazione).
* **Rapporto Segnale-Rumore ($\text{SNR}$):** misura la qualità del canale confrontando la potenza del segnale utile $S$ con la potenza del rumore $N$:
  $$\text{SNR}_{\text{dB}} = 10 \log_{10}\left(\frac{S}{N}\right)$$

---

## 4. Limiti Teorici della Trasmissione: [[Nyquist]] e [[Shannon]]
La massima velocità di trasmissione dati (**bit rate**, misurato in $\text{bps}$) su un canale fisico dipende dalla banda $B$ (in $\text{Hz}$) e dal rumore:

### A. Formula di Nyquist (Canale Ideale senza Rumore)
Dato un canale privo di rumore con banda $B$ e un segnale digitale a $M$ livelli discreti:

$$C_{\max} = 2 B \log_2(M) \quad [\text{bps}]$$

* Il termine $2B$ rappresenta il **baud rate** massimo (numero di simboli o variazioni di stato al secondo).

### B. Teorema di Shannon-Hartley (Canale Reale con Rumore)
In presenza di rumore gaussiano bianco, la capacità massima teorica $C$ di un canale di banda $B$ è limitata dal rapporto segnale-rumore $\text{SNR}$ (espresso in forma lineare, non in $\text{dB}$):

$$C = B \log_2(1 + \text{SNR}) \quad [\text{bps}]$$