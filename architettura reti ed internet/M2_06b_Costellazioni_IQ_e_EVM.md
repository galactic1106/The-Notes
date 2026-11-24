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
* Il segnale modulato digitale viene scomposto in due componenti ortogonali che non interferiscono tra loro:
  * **I (*In-phase*):** componente in fase (asse delle ascisse).
  * **Q (*Quadrature*):** componente in quadratura, sfasata di $90^\circ$ (asse delle ordinate).
* Nel piano cartesiano **IQ**, i punti ideali in cui può collocarsi il vettore del segnale formano una **costellazione** tanto più fitta quanto più è elevato l'ordine della modulazione.
* A causa del rumore e delle non linearità del canale, al momento della decisione il simbolo ricevuto non cade esattamente sul punto teorico ma risulta spostato, formando una **"nuvola" di punti** attorno a ciascuna posizione ideale.

---

## 2. Vettore Errore ed [[EVM]] (*Error Vector Magnitude*)
* **Vettore errore:** differenza vettoriale nel piano IQ tra il punto ideale di riferimento e il punto effettivamente misurato dal ricevitore. È caratterizzato da due componenti:
  * **Errore di ampiezza** (*IQ error mag*).
  * **Errore di fase** (*IQ error phase*, angolo $\phi$).
* **EVM (*Error Vector Magnitude*):** parametro principale per valutare la qualità intrinseca di una modulazione digitale, ricavato dal valore quadratico medio (RMS) dei moduli dei vettori errore misurati su più simboli:
  * **Espressione in percentuale (%):** rapporto tra l'errore RMS e l'ampiezza del segnale ideale; valori percentuali **più alti** indicano una qualità **peggiore**.
  * **Espressione in decibel (dB):** rapporto logaritmico tra la potenza media del vettore errore e quella del segnale ideale. Poiché nei sistemi funzionanti l'errore è inferiore al segnale utile, l'EVM in dB assume un **valore negativo** (tanto più negativo quanto migliore è la qualità del segnale).



