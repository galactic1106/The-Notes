---
corso: Architettura Reti ed Internet
modulo: Modulo 2 - Prof. Damiano Perri
università: Università degli Studi di Perugia (UNIPG)
argomento: Dettagli sulla quantizzazione PCM e modulazioni digitali (ASK, FSK, PSK, DPSK, QAM, Trellis)
tags:
  - reti
  - pcm
  - quantizzazione
  - modulazioni-digitali
  - ask-fsk-psk
  - trellis
---

# 📡 Quantizzazione PCM e Modulazioni Digitali

## 1. Approfondimento sulla Quantizzazione [[PCM]]
Nella conversione PCM, dopo il campionamento (secondo il teorema di Shannon $f_c \ge 2f_2$), la **quantizzazione** discretizza l'ampiezza del segnale in un numero finito di livelli compresi tra un minimo e un massimo prestabiliti:
* **Quantizzazione uniforme:** tutti i sotto-intervalli hanno la medesima ampiezza. Dato $n$ il numero di bit utilizzati:
  * Numero di livelli: $M = 2^n$
  * Ampiezza di ciascun livello: $q = \frac{V_{pp}}{M}$
  * Varianza dell'errore: $\sigma^2 = \frac{q^2}{12}$
* **Quantizzazione non uniforme (reale):** nelle reti reali l'ampiezza dei livelli segue una funzione **logaritmica**.
* **Errore di quantizzazione:** poiché la discretizzazione approssima infiniti valori analogici in un insieme finito, il processo è **irreversibile** e introduce un errore di quantizzazione; con $256\text{ livelli}$ ($8\text{ bit}$) l'orecchio umano non percepisce differenze sostanziali.

---

## 2. Modulazioni Digitali
Nella trasmissione dati tra due DTE, i dati binari ($0$ e $1$, oppure $-1$ e $+1$) possono essere convertiti in un diverso segnale digitale (codifica in **banda base**, utile per eliminare componente continua e basse frequenze) oppure modulati su una portante analogica:

### ASK (*Amplitude-Shift Keying*)
* Varia l'ampiezza della portante sinusoidale in funzione del bit modulante.
* **OOK (*On-Off Keying*):** caso più comune in cui lo $0$ logico corrisponde ad ampiezza nulla e l'$1$ logico all'ampiezza della portante non modulata.
* Semplice da implementare (usata in telescriventi e ponti radio a breve distanza), ma molto sensibile al rumore sulle lunghe distanze.

### FSK (*Frequency-Shift Keying*)
* Associa a ciascun valore logico una distinta frequenza compresa nella banda passante del mezzo (es. $f_A$ per l'$1$ e $f_B > f_A$ per lo $0$).
* La scelta delle frequenze bilancia **quattro requisiti**:
  1. Limitare l'occupazione di banda.
  2. Garantire sufficiente separazione tra le frequenze per evitare interferenza intersimbolica.
  3. Mantenere il periodo delle sinusoidi $\le$ alla durata di bit.
  4. Assicurare **continuità di fase** nelle transizioni di stato, indispensabile per il demodulatore (spesso un rivelatore di passaggio per lo zero).
* Impiegata nei primi modem (V.21, V.23), nei ponti radio e nella telefonia mobile **GSM**; su linea telefonica in rame permette comunicazioni **Full-Duplex** separando le bande delle due direzioni (es. $1070\text{–}1270\text{ Hz}$ in un senso e $2025\text{–}2225\text{ Hz}$ nell'altro).

### PSK (*Phase-Shift Keying*) e DPSK (*Differential PSK*)
* **2-PSK (BPSK):** mantiene ampiezza e frequenza costanti e commuta la fase tra due valori opposti ($0^\circ$ per l'$1$ e $180^\circ$ per lo $0$) tramite un modulatore bilanciato, massimizzando la protezione dal rumore.
* **DPSK:** per evitare la complessità di mantenere una fase di riferimento assoluta ($0^\circ$) in ricezione, l'informazione è codificata nei **salti di fase** rispetto al simbolo precedente (preceduta da una sequenza iniziale prestabilita):
  * *2-DPSK:* $0 = \text{nessuna variazione}$, $1 = \text{salto di } 180^\circ$.
  * *4-DPSK:* $00 = 0^\circ$, $01 = 90^\circ$, $10 = 180^\circ$, $11 = -90^\circ$ (o $+270^\circ$).

![[PSK_DPSK.md|1000]]    

### QAM e Codifica [[Trellis]]
* **16-QAM (Raccomandazione CCITT V.29):** raggruppa i bit in **quadribit** ($4\text{ bit}$ per simbolo, quadruplicando il *data rate* rispetto al *baud rate*, es. $9600\text{ bps}$ a $2400\text{ baud}$); gli ultimi 3 bit determinano $2^3 = 8$ salti di fase (regola 8-DPSK) mentre il primo bit seleziona tra $2$ livelli di ampiezza ($3\text{ V}$ o $5\text{ V}$ sugli assi principali, $\sqrt{2}\text{ V}$ o $3\sqrt{2}\text{ V}$ sulle diagonali).
* **64-QAM:** modulazione bidimensionale ottenuta combinando due PAM su portanti ortogonali (seno e coseno), impiegata nelle linee **ADSL** ($6\text{ bit}$ per simbolo).
* **Codifica Trellis:** modulazione con memoria rappresentabile tramite una macchina a due stati ($S_0 \to -A$ e $S_1 \to +A$) su un diagramma a traliccio (*trellis*), in cui il bit $0$ mantiene lo stato corrente e il bit $1$ provoca la transizione all'altro stato.

![[QAM_TRELLIS.md|1000]]    

