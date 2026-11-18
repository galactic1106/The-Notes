---
corso: Architettura Reti ed Internet
modulo: Modulo 2 - Prof. Damiano Perri
università: Università degli Studi di Perugia (UNIPG)
argomento: Rumore, rapporto SNR, interferenza costruttiva/distruttiva, ISI e modelli di canale
tags:
  - reti
  - rumore
  - snr
  - interferenza
  - isi
  - livello-fisico
---

# 📉 Rumore, SNR, Interferenza e Modelli di Canale

## 1. Rumore e Rapporto Segnale/Rumore ([[SNR]])
* A differenza della distorsione (che altera la forma d'onda per ritardi differenziali in frequenza ed è intrinseca al sistema)[cite: 15], il **rumore** è un disturbo aleatorio che si sovrappone al segnale rispetto al rumore di fondo (*noise floor*)[cite: 15].
* **SNR (*Signal-to-Noise Ratio*):** misura in **dB** il margine tra la potenza del segnale ricevuto e il *noise floor*[cite: 15].

### Valori di riferimento su linea ADSL (fino a $8\text{ Mbps}$)
| Valore SNR | Qualità della linea |
| :--- | :--- |
| **$\le 6\text{ dB}$** | Segnale assente o intermittente[cite: 15] |
| **$7\text{ – }10\text{ dB}$** | Pessima qualità, connessione instabile[cite: 15] |
| **$11\text{ – }15\text{ dB}$** | Segnale sulla soglia (verificare impianto e filtri)[cite: 15] |
| **$16\text{ – }20\text{ dB}$** | Segnale pulito, nessuna anomalia[cite: 15] |
| **$20\text{ – }28\text{ dB}$** | Segnale ottimo[cite: 15] |
| **$> 28\text{ dB}$** | Segnale eccellente (molto vicini alla centrale)[cite: 15] |

*(Nota: un'attenuazione di linea superiore a **$60\text{ dB}$** comporta prestazioni pessime e disconnessioni frequenti)[cite: 15].*

---

## 2. Interferenza ed [[ISI]] (*Inter Symbol Interference*)
* **Interferenza:** sovrapposizione di segnali esterni indesiderati che alterano il segnale utile[cite: 15]:
  * **Interferenza costruttiva:** due onde **in fase** si sommano amplificando l'ampiezza risultante[cite: 15].
  * **Interferenza distruttiva:** due onde **sfasate** (in opposizione di fase) si annullano reciprocamente[cite: 15].
* **Interferenza Intersimbolica (ISI):** fenomeno causato dalle **limitazioni di banda** del canale, che arrotonda gli impulsi rettangolari emessi dal DTE facendoli sconfinare negli intervalli temporali ($T$) dei simboli adiacenti[cite: 15].

---

## 3. Classificazione dei Canali Trasmissivi
Per analizzare i limiti della velocità di trasferimento (misurata in $\text{bit/s}$) si distinguono due modelli teorici di riferimento[cite: 15]:
* **Canale perfetto:** trasmette il segnale senza alcuna distorsione né ritardo di propagazione[cite: 15].
* **Canale ideale:** non introduce distorsioni, ma applica un **ritardo di propagazione costante** a tutte le componenti del segnale[cite: 15].