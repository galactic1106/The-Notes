---
corso: Architettura Reti ed Internet
modulo: Modulo 2 - Prof. Damiano Perri
università: Università degli Studi di Perugia (UNIPG)
argomento: Segnali sinusoidali, sviluppo in serie di Fourier e filtri
tags:
  - reti
  - segnali
  - fourier
  - filtri
  - livello-fisico
---

# 〰️ Segnali, Serie di Fourier e Filtri

## 1. Segnali Analogici, Digitali e Sinusoidali
* **Segnale analogico:** assume per definizione infiniti livelli di intensità nel tempo.
* **Segnale digitale:** assume un numero finito di livelli (tipicamente $0$ e $1$).

### Parametri di un Segnale Sinusoidale
Un segnale sinusoidale varia nel tempo secondo la legge:

$$u(t) = U \sin(\omega t + \varphi)$$

* **$U$**: ampiezza massima del segnale.
* **$\omega = 2\pi f$**: velocità angolare (pulsazione).
* **$\varphi$**: sfasamento o fase iniziale all'istante $t = 0$.
* **Frequenza ($f = 1/T$)**: numero di cicli completi al secondo, misurata in **Hertz ($\text{Hz}$)**.
* **Periodo ($T = 1/f$)**: durata temporale di una singola oscillazione completa.
* **Lunghezza d'onda ($\lambda = c / f$)**: distanza spaziale tra due massimi consecutivi, dove $c$ è la velocità di propagazione del segnale ($300.000\text{ km/s}$ nel vuoto).

---

## 2. Sviluppo in Serie di [[Fourier]]
Secondo il **Teorema di Fourier**, qualsiasi segnale periodico può essere scomposto nella somma di infinite sinusoidi (armoniche) con ampiezze, frequenze e fasi opportune:

$$S(t) = A_0 + A_1\sin(\omega t + \varphi_1) + A_2\sin(2\omega t + \varphi_2) + A_3\sin(3\omega t + \varphi_3) + \dots$$

* Sommando un numero crescente di armoniche dispari (fondamentale, terza, quinta, settima, nona...) si approssima con precisione sempre maggiore un'onda quadra (segnale digitale).
* Un segnale digitale ideale ha una **banda teoricamente infinita**; nella pratica si considerano le armoniche fino a quando la loro ampiezza scende a un decimo della componente fondamentale.
* **Modalità di trasmissione:** in **banda base** (trasmissione diretta del segnale) oppure in **banda traslata** (mediante modulazione).

---

## 3. I [[Filtri]]
Un **filtro** è un circuito che lascia passare o attenua specifiche bande di frequenza, permettendo di separare segnali diversi o eliminare disturbi fuori banda.
* **Filtri passivi:** realizzati solo con componenti passivi (resistori $R$, condensatori $C$).
* **Filtri attivi:** impiegano anche componenti attivi (amplificatori operazionali, transistori).

### Tipologie di Filtri
1. **Filtro Passa-Basso (*Low-Pass*):** lascia passare le frequenze inferiori alla **frequenza di taglio** ($f_t$, dove $V_{\text{out}}/V_{\text{in}} = 1/\sqrt{2} \approx 0,707$, pari a $-3\text{ dB}$ ovvero dimezzamento della potenza) e attenua quelle superiori. Realizzabile con un circuito $RC$ in serie.
2. **Filtro Passa-Alto (*High-Pass*):** lascia passare le frequenze superiori alla frequenza di taglio e attenua quelle inferiori (circuito $CR$ in serie).
3. **Filtro Passa-Banda (*Band-Pass*):** lascia passare solo le frequenze comprese in un intervallo (*banda passante* $BW$) intorno alla frequenza centrale $f_0$, attenuando quelle esterne (ottenibile combinando un passa-basso e un passa-alto).
4. **Filtro Elimina-Banda (*Band-Stop* / *Notch*):** blocca le frequenze comprese in un determinato intervallo $[\omega_L, \omega_H]$ lasciando passare tutte le altre.





