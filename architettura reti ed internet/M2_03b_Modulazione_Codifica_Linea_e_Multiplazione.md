---
corso: Architettura Reti ed Internet
modulo: Modulo II - Prof. Damiano Perri
fonte: parte_3.pdf
argomento: Codifiche di linea, modulazione analogica/digitale, PCM, modem e multiplazione
tags:
  - reti
  - modulo-2
  - perri
  - modulazione
  - multiplazione
  - modem
  - pcm
  - manchester
---

# 🎛️ Modulazione, Codifica di Linea e Multiplazione

## 1. Adattamento dei Dati al Canale (4 Combinazioni)
A seconda della natura dei dati sorgente e del tipo di canale trasmissivo, si applicano tecniche diverse:

| Dati Sorgente | Segnale sul Canale | Tecnica Utilizzata | Esempi Tipici |
| :--- | :--- | :--- | :--- |
| **Digitali** | **Digitale** | **Codifica di linea** (*Line Coding*) | Ethernet cablato, bus interni, linee seriali |
| **Digitali** | **Analogico** | **Modulazione digitale** (Modem) | Reti Wi-Fi, ADSL, reti cellulari, modem telefonici |
| **Analogici** | **Digitale** | **Digitalizzazione / Codec** ([[PCM]]) | Telefonia digitale ISDN/VoIP, audio CD |
| **Analogici** | **Analogico** | **Modulazione analogica** | Radio AM/FM, TV analogica |

---

## 2. Trasmissione Digitale su Canale Digitale (Codifiche di Linea)
* **NRZ (*Non-Return-to-Zero*):** associa due livelli di tensione costanti ai bit `0` e `1` (es. $-V$ e $+V$). Semplice ma soffre di **perdita di sincronismo** in presenza di lunghe sequenze consecutive di `0` o di `1` e presenta una componente continua (*DC*).
* **Codifica [[Manchester]]:** ogni intervallo di bit prevede una **transizione a metà periodo** (es. transizione basso-alto per `1`, alto-basso per `0` secondo IEEE 802.3). Garantisce **autosincronizzazione** continua del clock e assenza di componente continua, al prezzo di richiedere il doppio della banda (2 variazioni di livello per bit).
* **Manchester Differenziale:** la presenza o assenza di transizione all'inizio dell'intervallo di bit codifica il valore (`0` o `1`), mentre la transizione a metà periodo serve esclusivamente per il clock.

---

## 3. Modulazione (Dati Digitali su Segnale Analogico) e [[Modem]]
Quando il mezzo trasmissivo richiede un segnale analogico (es. linea telefonica o etere radio), un **Modem** (*MOdulatore / DEModulatore*) varia uno o più parametri di un'onda sinusoidale ad alta frequenza detta **portante** (*carrier*):

$$s(t) = A \sin(2\pi f t + \phi)$$

* **[[ASK]] (*Amplitude Shift Keying*):** varia l'**ampiezza** $A$ della portante in base ai bit da trasmettere (sensibile ai disturbi di ampiezza).
* **[[FSK]] (*Frequency Shift Keying*):** commuta tra due o più **frequenze** $f_1, f_2$ della portante (molto robusta contro il rumore, usata nei modem a bassa velocità).
* **[[PSK]] (*Phase Shift Keying*):** varia la **fase** $\phi$ della portante (es. BPSK con 2 fasi a $180^\circ$, QPSK con 4 fasi a $90^\circ$ che trasporta 2 bit per simbolo).
* **[[QAM]] (*Quadrature Amplitude Modulation*):** combina simultaneamente variazioni di **ampiezza e fase** (es. 16-QAM, 64-QAM, 256-QAM), consentendo di trasmettere molti bit per singolo simbolo (*alta efficienza spettrale*) nelle reti Wi-Fi, 4G/5G e xDSL.

![[grafi_modulazioni_digitali_ask_fsk_psk_e_diagramma_a_costellazione_iq|1000]]

### Digitalizzazione di Segnali Analogici: [[PCM]] (*Pulse Code Modulation*)
Utilizzata dai **Codec** per convertire la voce analogica in flussi digitali:
1. **Campionamento (*Sampling*):** si prelevano campioni del segnale analogico a intervalli regolari. Per il **Teorema del Campionamento di Nyquist-Shannon**, la frequenza di campionamento $f_s$ deve essere almeno il doppio della frequenza massima $f_{\max}$ del segnale:
   $$f_s \ge 2 f_{\max}$$
   *(Esempio telefonia: banda voce $0\text{–}4000\text{ Hz} \implies f_s = 8000\text{ campioni/s}$.)*
2. **Quantizzazione:** ciascun campione viene approssimato al valore discreto più vicino tra $L = 2^n$ livelli (con $n=8\text{ bit} \implies 256$ livelli, da cui $8000 \times 8 = 64\text{ kbps}$).
3. **Codifica:** ogni livello quantizzato viene tradotto nella corrispondente parola binaria a $n$ bit.

---

## 4. Tecniche di Multiplazione (*Multiplexing*)
La **multiplazione** consente a più flussi di comunicazione indipendenti di condividere simultaneamente un unico mezzo trasmissivo ad alta capacità tramite un **MUX** (multiplatore) in trasmissione e un **DEMUX** (demultiplatore) in ricezione:

1. **[[FDM]] (*Frequency Division Multiplexing*):**
   * La banda complessiva del canale viene suddivisa in **sotto-bande di frequenza disgiunte**, separate da bande di guardia (*guard bands*) per evitare interferenze.
   * Ogni flusso occupa continuamente la propria frequenza dedicata (es. trasmissioni radio/TV, ADSL).
2. **[[TDM]] (*Time Division Multiplexing*):**
   * L'intera banda del canale è assegnata a turno a ciascun flusso per brevi intervalli di tempo (**time slot**).
   * **TDM Sincrono:** gli slot sono assegnati in modo fisso (se un utente non trasmette, il suo slot resta vuoto).
   * **TDM Statistico (Asincrono):** gli slot vengono allocati dinamicamente solo alle sorgenti attive, ottimizzando la banda.
3. **[[WDM]] (*Wavelength Division Multiplexing*):**
   * Equivalente ottico dell'FDM applicato alla **fibra ottica**: più segnali laser a diverse **lunghezze d'onda** ($\lambda$, colori diversi) viaggiano contemporaneamente nella stessa fibra (**DWDM** – *Dense WDM*).
4. **[[CDMA]] (*Code Division Multiple Access*):**
   * Tutti gli utenti trasmettono contemporaneamente sull'intera banda di frequenza utilizzando **codici ortogonali** univoci che permettono al ricevitore di separare i singoli segnali (reti cellulari 3G).