---
corso: Architettura Reti ed Internet
modulo: Modulo 2 - Prof. Damiano Perri
università: Università degli Studi di Perugia (UNIPG)
argomento: Diagrammi a costellazione IQ, vettore errore ed EVM
tags:
  - reti
  - costellazione-iq
  - evm
  - vettore-errore
  - modulazioni-digitali
---

# 🎯 Diagrammi a Costellazione IQ ed EVM

## 1. Diagramma a Costellazione (Piano IQ)
* Il segnale modulato digitale viene scomposto in due componenti ortogonali che non interferiscono tra loro[cite: 13]:
  * **I (*In-phase*):** componente in fase (asse delle ascisse)[cite: 13].
  * **Q (*Quadrature*):** componente in quadratura, sfasata di $90^\circ$ (asse delle ordinate)[cite: 13].
* Nel piano cartesiano **IQ**, i punti ideali in cui può collocarsi il vettore del segnale formano una **costellazione** tanto più fitta quanto più è elevato l'ordine della modulazione[cite: 13].
* A causa del rumore e delle non linearità del canale, al momento della decisione il simbolo ricevuto non cade esattamente sul punto teorico ma risulta spostato, formando una **"nuvola" di punti** attorno a ciascuna posizione ideale[cite: 13].

---

## 2. Vettore Errore ed [[EVM]] (*Error Vector Magnitude*)
* **Vettore errore:** differenza vettoriale nel piano IQ tra il punto ideale di riferimento e il punto effettivamente misurato dal ricevitore[cite: 13]. È caratterizzato da due componenti[cite: 13]:
  * **Errore di ampiezza** (*IQ error mag*)[cite: 13].
  * **Errore di fase** (*IQ error phase*, angolo $\phi$)[cite: 13].
* **EVM (*Error Vector Magnitude*):** parametro principale per valutare la qualità intrinseca di una modulazione digitale, ricavato dal valore quadratico medio (RMS) dei moduli dei vettori errore misurati su più simboli[cite: 13]:
  * **Espressione in percentuale (%):** rapporto tra l'errore RMS e l'ampiezza del segnale ideale; valori percentuali **più alti** indicano una qualità **peggiore**[cite: 13].
  * **Espressione in decibel (dB):** rapporto logaritmico tra la potenza media del vettore errore e quella del segnale ideale[cite: 13]. Poiché nei sistemi funzionanti l'errore è inferiore al segnale utile, l'EVM in dB assume un **valore negativo** (tanto più negativo quanto migliore è la qualità del segnale)[cite: 13].