---
corso: Architettura Reti ed Internet
modulo: Modulo 2 - Prof. Damiano Perri
università: Università degli Studi di Perugia (UNIPG)
argomento: Modulazioni analogiche, impulsive, digitali e tecnica PCM
tags:
  - reti
  - modulazione
  - pcm
  - qam
  - psk
  - tdm
---

# 📻 Modulazioni e Pulse Code Modulation (PCM)

## 1. Modulazioni ad Onda Continua (Analogiche)
La **modulazione** è un processo reversibile in cui un parametro di un segnale ad alta frequenza (**portante**) viene variato in funzione del segnale informativo (**modulante**):

| Tipo                            | Parametro variato | Equazione / Caratteristiche principali                                                                                                                                                             |
| :------------------------------ | :---------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **AM** (*Amplitude Modulation*) | Ampiezza          | $v(t) = [V_p + K_a V_m \cos(\omega_m t)]\cos(\omega_p t)$. Molto sensibile al rumore perché l'informazione risiede nell'ampiezza.                                                                  |
| **FM** (*Frequency Modulation*) | Frequenza         | $v_{\text{FM}}(t) = V_p \sin(\omega_p t + m_f \sin(\omega_m t))$, con $m_f = \Delta f / f_m$. Ampiezza costante, molto più immune ai disturbi (usata nella radio FM $87,5\text{–}108\text{ MHz}$). |
| **PM** (*Phase Modulation*)     | Fase              | $v_{\text{PM}}(t) = V_p \cos(\omega_p t + K_p V_m \sin(\omega_m t))$, con $m_p = K_p V_m$. Modulazione angolare con frequenza portante più stabile rispetto alla FM.                               |

![[Modulazioni_ad_Onda_Continua|1000]]

---

## 2. Modulazioni Impulsive e Tecnica [[PCM]]
Nelle modulazioni impulsive l'informazione modifica un treno di impulsi portanti:
* **PAM** (*Pulse Amplitude Modulation*): varia l'**ampiezza** degli impulsi.
* **PWM** (*Pulse Width Modulation*): varia la **durata (larghezza)** degli impulsi.
* **PPM** (*Pulse Position Modulation*): varia la **posizione temporale** degli impulsi rispetto all'istante di riposo.

### [[PCM]] (*Pulse Code Modulation*)
Standard per la digitalizzazione e la multiplazione a divisione di tempo (**TDM**) dei canali telefonici su un unico mezzo:
1. **Campionamento:** in rete telefonica **PSTN** (banda vocale fino a $f_2 = 3,4\text{ kHz}$), per il teorema di Shannon ($f_c \ge 2f_2 = 6,8\text{ kHz}$) si campiona a **$f_c = 8\text{ kHz}$**, ovvero un campione ogni **$T = 1 / 8000 = 125\ \mu\text{s}$**.
2. **Quantizzazione:** ciascun campione PAM viene discretizzato su **$256\text{ livelli}$**.
3. **Codifica:** ogni livello quantizzato è codificato con **$8\text{ bit}$**.

#### Struttura della Trama PCM Europea (E1 – 32 canali)
* Ogni trama dura $125\ \mu\text{s}$ e trasporta **32 slot da 8 bit** ($30$ canali vocali utili + slot $0$ per il **sincronismo** + slot $16$ per **segnalazione/controllo**):
  $$V_{\text{bit}} = 32 \times 8000 \times 8 = 2.048\text{ kbit/s} \quad (2,048\text{ Mbit/s})$$
* I canali possono essere ulteriormente raggruppati in gerarchie superiori ($120, 480, 1920, 7680$ canali).

![[Modulazioni_Impulsive|1000]]  


---

## 3. Modulazioni Digitali (PSK e QAM)
* **4-PSK (QPSK):** il flusso binario viene diviso in coppie di bit (**dibit**) tramite un convertitore seriale/parallelo (bit pari e bit dispari) che modulano due portanti in quadratura (sfasate di $90^\circ$), generando 4 possibili fasi ($45^\circ, 135^\circ, 225^\circ, 315^\circ$).
* **16-QAM** (*Quadrature Amplitude Modulation*): raggruppa i bit in **quadribit** ($4\text{ bit}$ per simbolo) combinando **8 salti di fase** (codificati dagli ultimi 3 bit secondo la regola 8-DPSK) e **2 livelli di ampiezza** (determinati dal primo bit), ottenendo $2^4 = 16$ stati distinti.

![[Modulazioni_Digitali|1000]] 








