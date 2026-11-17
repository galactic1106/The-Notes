---
corso: Architettura Reti ed Internet
modulo: Modulo II - Prof. Damiano Perri
fonte: parte_2.pdf
argomento: Enti di standardizzazione, modello ISO/OSI vs TCP/IP e Reti Mesh
tags:
  - reti
  - modulo-2
  - perri
  - iso-osi
  - tcp-ip
  - standardizzazione
  - reti-mesh
---

# 🏛️ Standardizzazione, Modello ISO/OSI e Reti Mesh

## 1. Gli Enti di Standardizzazione nelle Telecomunicazioni
Gli standard garantiscono l'interoperabilità tra apparati hardware e software di produttori diversi. I principali organismi internazionali sono:

* **[[ISO]] (*International Organization for Standardization*):** organizzazione internazionale che stabilisce standard in diversi settori, inclusi i sistemi di comunicazione (modello di riferimento OSI) e la sicurezza informatica.
* **[[IEEE]] (*Institute of Electrical and Electronics Engineers*):** associazione professionale che definisce standard tecnici per l'elettronica e le reti locali/metropolitane (famiglia **IEEE 802**: 802.3 Ethernet, 802.5 Token Ring, 802.11 Wi-Fi, 802.16 WiMAX).
* **[[ITU]] (*International Telecommunication Union*, ex CCITT):** agenzia delle Nazioni Unite che coordina gli standard globali per le telecomunicazioni, le reti telefoniche e l'allocazione dello spettro radio.
* **[[W3C]] (*World Wide Web Consortium*):** consorzio internazionale che sviluppa standard aperti per il Web (HTML, CSS, XML, ecc.).
* **[[ICANN]] (*Internet Corporation for Assigned Names and Numbers*):** ente che coordina a livello globale l'assegnazione degli identificativi univoci di Internet, inclusi i nomi a dominio (**[[DNS]]**) e gli spazi di indirizzamento **[[IP]]**.

---

## 2. Modello di Riferimento [[ISO-OSI]] vs Stack [[TCP-IP]]
Il modello **ISO/OSI** (*Open Systems Interconnection*) scompone l'architettura di rete in **7 livelli**, suddivisi in due macro-categorie:

* **Host Layers (Livelli 4–7):** gestiti dagli host terminali (*end systems*).
* **Media Layers (Livelli 1–3):** legati all'infrastruttura fisica e di instradamento (*intermediate systems* come switch e router).

| # | Livello ISO/OSI | Categoria | Unità Dati (PDU) | Funzione Chiave | Livello TCP/IP Corrispondente |
| :-: | :--- | :--- | :--- | :--- | :--- |
| **7** | **Application** | Host Layer | Data | Interfaccia verso i processi applicativi di rete. | **Application Layer** |
| **6** | **Presentation** | Host Layer | Data | Rappresentazione dei dati, codifica, compressione e cifratura. | **Application Layer** |
| **5** | **Session** | Host Layer | Data | Apertura, gestione e chiusura delle sessioni tra host. | **Application Layer** |
| **4** | **Transport** | Host Layer | **Segments** | Connessioni *end-to-end*, controllo di flusso e affidabilità (TCP/UDP). | **Transport Layer** |
| **3** | **Network** | Media Layer | **Packets** | Determinazione del percorso (*routing*) e indirizzamento logico (**IP**). | **Internet Layer** |
| **2** | **Data Link** | Media Layer | **Frames** | Indirizzamento fisico (**MAC**), sottolivelli **LLC** e **MAC**, controllo errori sul link. | **Network Access Layer** |
| **1** | **Physical** | Media Layer | **Bits** | Mezzo trasmissivo, codifica del segnale e trasmissione binaria. | **Network Access Layer** |

> [!note] Inoltro nei sistemi intermedi
> Durante la comunicazione tra due host attraverso una rete di router, i nodi intermedi elaborano esclusivamente i **Media Layers (Livelli 1, 2 e 3)** per inoltrare le PDU incapsulate fino al destinatario.

---

## 3. Reti Mesh (*Wireless Mesh Networks*)
* **Criticità del mezzo radio:** il canale wireless è soggetto a un elevato tasso di errore, attenuazione e interferenze, rendendo indispensabili protocolli di routing specificamente ottimizzati per il mondo wireless.
* **Wireless Backhaul Multihop:** la maglia interconnessa realizzata tra **wireless router** e **access point** forma un'infrastruttura *multihop* (a salti multipli).
* **Vantaggio architetturale:** consente di fornire agli utenti mobili servizi di interconnessione a banda larga e a basso costo utilizzando un numero ridotto di punti di accesso cablati (*gateway*) collegati direttamente a Internet.