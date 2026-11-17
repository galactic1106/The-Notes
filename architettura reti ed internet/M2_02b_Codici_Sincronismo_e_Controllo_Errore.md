---
corso: Architettura Reti ed Internet
modulo: Modulo II - Prof. Damiano Perri
fonte: parte_2.pdf
argomento: Codifica dei caratteri, trasmissione sincrona e asincrona, rilevazione e correzione degli errori
tags:
  - reti
  - modulo-2
  - perri
  - codifica
  - controllo-errore
  - crc
  - hamming
---

# 🔢 Codici, Sincronismo e Controllo dell'Errore

## 1. Codifica dell'Informazione
Per trasmettere testi, numeri e comandi su un sistema digitale è necessario associare a ciascun simbolo una sequenza binaria univoca tramite un **codice**:

* **[[ASCII]] (*American Standard Code for Information Interchange*):** codice standard a **7 bit** ($2^7 = 128$ caratteri, tra cui lettere, cifre, punteggiatura e caratteri di controllo come `STX`, `ETX`, `ACK`, `NAK`, `DLE`), spesso esteso a **8 bit** (*Extended ASCII*, 256 simboli).
* **[[EBCDIC]] (*Extended Binary Coded Decimal Interchange Code*):** codice proprietario IBM a **8 bit** utilizzato storicamente nei sistemi mainframe.
* **[[Unicode]] (UTF-8, UTF-16):** standard universale progettato per rappresentare tutti i sistemi di scrittura mondiali.

---

## 2. Trasmissione Seriale vs Parallela e Sincronismo
* **Trasmissione Parallela:** più bit vengono inviati contemporaneamente su altrettanti fili distinti (veloce su brevi distanze, soggetta a *skew* e diafonia su lunghe tratte).
* **Trasmissione Seriale:** i bit vengono inviati uno dopo l'altro su un unico canale. Richiede sincronizzazione tra trasmettitore e ricevitore:

### A. Trasmissione Asincrona
* I dati vengono inviati un carattere alla volta (es. 7 o 8 bit) incapsulato tra un **bit di Start** (transizione da stato di riposo `1` a `0`) e uno o più **bit di Stop** (`1`), con eventuale bit di parità.
* Il ricevitore risincronizza il proprio clock ad ogni bit di Start.
* **Pro/Contro:** semplice ed economica, ma introduce un elevato *overhead* (2–3 bit di controllo ogni 8 bit utili).

### B. Trasmissione Sincrona
* I bit vengono trasmessi in blocchi continui (*frame*) senza bit di Start/Stop per singolo carattere.
* La sincronizzazione è mantenuta tramite una **linea di clock dedicata** oppure estraendo il clock direttamente dal segnale codificato (es. codifica **Manchester**) e utilizzando sequenze iniziali di sincronismo (**preambolo / flag**).
* **Pro/Contro:** efficienza molto elevata per grandi volumi di dati, ma richiede circuiti di sincronizzazione più complessi.

---

## 3. Rilevazione e Correzione degli Errori
Il rumore sul canale trasmissivo può alterare uno o più bit (**errori singoli** o **errori a raffica / *burst***). Per contrastarli si aggiunge **ridondanza** ai dati trasmessi.

### A. Bit di Parità (*Parity Check*)
* Si aggiunge 1 bit ridondante affinché il numero totale di `1` nella parola sia **pari** (*even parity*) o **dispari** (*odd parity*).
* **Limite:** rileva tutti gli errori su un numero dispari di bit, ma non rileva errori su un numero pari di bit (es. 2 bit invertiti si compensano).
* **Parità bidimensionale (LRC/VRC):** organizza i bit in una matrice calcolando la parità per ogni riga e per ogni colonna; consente di individuare e correggere un singolo bit errato.

### B. [[CRC]] (*Cyclic Redundancy Check*)
Tecnica ad altissima affidabilità basata sull'aritmetica polinomiale modulo 2 (senza riporti, operazione XOR):
1. Si rappresenta il messaggio binario come un polinomio $M(x)$ e si sceglie un **polinomio generatore $G(x)$** di grado $r$ noto a trasmettitore e ricevitore.
2. Si moltiplica $M(x)$ per $x^r$ (aggiungendo $r$ zeri in coda) e si divide $x^r M(x)$ per $G(x)$ ottenendo il resto $R(x)$ di $r$ bit (**FCS** – *Frame Check Sequence*).
3. Si trasmette $T(x) = x^r M(x) + R(x)$, che risulta esattamente divisibile per $G(x)$.
4. Il ricevitore divide il blocco ricevuto per $G(x)$: se il resto è **zero** il frame è accettato, altrimenti viene rilevato un errore.

### C. Codici a Correzione d'Errore ([[FEC]]) e Distanza di [[Hamming]]
* **Distanza di Hamming $d(x,y)$:** numero di bit in cui due parole di codice valide differiscono. La distanza minima $d_{\min}$ del codice determina le sue capacità:
  * **Rilevazione** fino a $d$ errori: $d_{\min} \ge d + 1$
  * **Correzione** fino a $t$ errori: $d_{\min} \ge 2t + 1$
* **Codice di Hamming:** inserisce bit di parità nelle posizioni corrispondenti alle potenze di $2$ ($1, 2, 4, 8, \dots$); al ricevitore, il calcolo della **sindrome** identifica l'esatta posizione del singolo bit errato consentendone l'inversione automatica senza ritrasmissione.