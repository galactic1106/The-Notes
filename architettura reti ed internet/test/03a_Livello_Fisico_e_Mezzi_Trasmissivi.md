---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_c.pdf
argomento: Livello Fisico (Physical Layer) ISO/OSI e mezzi trasmissivi cablati
tags:
  - reti
  - physical-layer
  - mezzi-trasmissivi
  - doppino
  - cavo-coassiale
  - fibra-ottica
---

# ⚡ Livello Fisico (*Physical Layer*) e Mezzi Trasmissivi

## 1. Ruolo e Funzioni del Livello Fisico (Livello 1 ISO/OSI)
Il primo livello del modello ISO/OSI si occupa della **trasmissione di flussi di bit grezzi** (*raw bits*) su un canale di comunicazione fisico, definendo le caratteristiche **meccaniche, elettriche, funzionali e procedurali** per attivare, mantenere e disattivare la connessione fisica tra sistemi.

* **Rappresentazione dei bit:** trasforma i bit ricevuti dal livello Data Link in segnali fisici idonei al mezzo:
  * **Cavi in rame:** impulsi elettrici.
  * **Fibra ottica:** impulsi luminosi.
  * **Wireless:** onde elettromagnetiche (microonde / radiofrequenza).
* **Standard di interfaccia fisica (*Physical Link*):** definiscono le regole di interconnessione per terminali, modem e schede di rete (es. **RS-232**, **V.24**, **V.35**, **SONET/SDH**).

---

## 2. Mezzi Trasmissivi Guidati (Cablati)

### A. Doppino Ritorto (*Twisted Pair*)
Composto da coppie di conduttori in rame isolati e intrecciati tra loro per ridurre le interferenze elettromagnetiche (*crosstalk* / diafonia). È il mezzo standard nella telefonia e nelle reti **[[Ethernet]]**.
* **[[UTP]] (*Unshielded Twisted Pair*):** privo di schermatura metallica; economico e flessibile, ma sensibile ai disturbi di macchine elettriche e luci fluorescenti (agisce da antenna all'aumentare della lunghezza). Per impianti certificati si usano cavi almeno di **Cat. 5 / 5e**.
* **[[STP]] (*Shielded Twisted Pair*):** dotato di schermatura metallica contro i disturbi esterni; più costoso e rigido, indicato per ambienti rumorosi o velocità elevate (es. Gigabit Ethernet).
* **Categorie principali:** Cat. 5, Cat. 5e, Cat. 6, Cat. 7 (al crescere della categoria aumentano banda passante, rigidità e affidabilità).

### B. Cavo Coassiale (*Coaxial Cable*)
Costituito da un conduttore centrale in rame, uno strato isolante (**dielettrico**), una calza schermante in rame e una guaina esterna in polietilene.
* **Impedenze standard:**
  * **$50\,\Omega$:** trasmissione dati e strumentazione di misura.
  * **$75\,\Omega$:** segnali televisivi analogici e digitali.
* **Varianti storiche Ethernet ($50\,\Omega$):**
  * **Thick Ethernet (RG-8, $0.4"$):** utilizza un *transceiver* collegato alla scheda di rete tramite cavo a 9 fili (max $50\,\text{m}$) e connettore **AUI a 15 pin**.
  * **Thin Ethernet (RG-58, $0.25"$):** utilizza connettori **BNC a T** con innesto a baionetta.

### C. Fibra Ottica (*Fiber Optic*)
Trasmette informazioni sotto forma di **impulsi luminosi** (generati da LED o laser) attraverso un nucleo vetroso (*core*) circondato da un rivestimento (*cladding*) con indice di rifrazione inferiore, sfruttando il principio della **riflessione interna totale**.
* **Vantaggi:** immunità totale ai disturbi elettromagnetici, attenuazione bassissima su lunghe distanze, elevatissima larghezza di banda e sicurezza contro le intercettazioni.
* **Tipologie:**
  * **Multimodale (*Multi-Mode*):** core più largo, più raggi luminosi con diversi angoli di incidenza; adatta a tratte brevi/medie (es. dorsali di edificio o campus).
  * **Monomodale (*Single-Mode*):** core sottilissimo attraversato da un unico modo di propagazione; ideale per dorsali geografiche e collegamenti a lunghissima distanza.