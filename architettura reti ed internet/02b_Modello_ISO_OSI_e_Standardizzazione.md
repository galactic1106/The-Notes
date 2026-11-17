---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_b.pdf
argomento: Modello di riferimento ISO/OSI, incapsulamento, sistemi intermedi ed enti di standardizzazione
tags:
  - reti
  - iso-osi
  - protocolli
  - standardizzazione
  - tcp-ip
---

# 🏛️ Modello ISO/OSI ed Enti di Standardizzazione

## 1. Architettura a Livelli e Modello [[ISO-OSI]]
Per gestire la complessità delle reti di calcolatori, le architetture di comunicazione sono organizzate in **livelli (layer) sovrapposti**. Il modello di riferimento **OSI** (*Open Systems Interconnection*), definito dall'**ISO**, articola la comunicazione su **7 livelli**:

| Livello | Nome (EN / IT) | Funzione Principale | Unità Dati |
| :--- | :--- | :--- | :--- |
| **7** | **Application** (Applicazione) | Interfaccia diretta con l'utente e i software applicativi (HTTP, SMTP, DNS, FTP) | Messaggio / Dati |
| **6** | **Presentation** (Presentazione) | Sintassi e semantica dei dati: codifica, compressione e crittografia (SSL/TLS, ASCII, JPEG) | Dati |
| **5** | **Session** (Sessione) | Apertura, gestione, sincronizzazione e chiusura del dialogo tra applicazioni | Dati |
| **4** | **Transport** (Trasporto) | Comunicazione **end-to-end** tra processi, controllo di flusso, segmentazione e affidabilità (TCP, UDP) | **Segmento** / Datagram |
| **3** | **Network** (Rete) | Indirizzamento logico globale (**IP**) e instradamento (**routing**) tra reti diverse | **Pacchetto** / Datagram |
| **2** | **Data Link** (Collegamento) | Accesso al mezzo condiviso, indirizzamento fisico (**MAC**), framing e controllo errori su singolo link | **Frame** (Trama) |
| **1** | **Physical** (Fisico) | Trasmissione dei singoli **bit** grezzi sotto forma di segnali elettrici, ottici o radio | **Bit** |

---

## 2. Concetti Chiave dell'Architettura a Strati
* **Entità (*Entity*):** elementi attivi all'interno di ciascun livello (processi software o componenti hardware). Le entità allo stesso livello su nodi diversi sono dette **entità pari (*peer entities*)**.
* **Protocollo:** insieme di regole e formati che governano la comunicazione orizzontale (logica) tra entità pari di livello $N$.
* **Interfaccia e [[SAP]] (*Service Access Point*):** confine tra livello $N$ e livello $N-1$ sullo stesso nodo; il livello $N-1$ offre servizi al livello superiore $N$ attraverso i punti di accesso **SAP**.
* **Incapsulamento (SDU, PCI, PDU):**
  * **SDU (*Service Data Unit*):** i dati provenienti dal livello superiore $N+1$ che devono essere trasportati.
  * **PCI (*Protocol Control Information*):** l'intestazione (*header*, ed eventualmente *trailer*) aggiunta dal livello $N$ per gestire il proprio protocollo.
  * **PDU (*Protocol Data Unit*):** l'unità dati complessiva del livello $N$, data da $\text{PDU}_N = \text{PCI}_N + \text{SDU}_N$, che diventa a sua volta la $\text{SDU}_{N-1}$ del livello sottostante.
* **Sistemi Terminali (*End Systems*) vs Sistemi Intermedi (*Intermediate Systems*):**
  * Gli **host** sorgente e destinazione implementano tutti e 7 i livelli.
  * I **sistemi intermedi** (es. switch a livello 2, router a livello 3) elaborano solo i livelli inferiori necessari all'inoltro.

---

## 3. Confronto tra Modello ISO/OSI e Stack [[TCP-IP]]
Il modello **TCP/IP** (nato prima del modello OSI dalla pratica operativa di ARPANET) condensa i 7 livelli OSI in **4 livelli** (o 5 nei modelli ibridi):
1. **Application (Applicazione):** accorpa i livelli 5, 6 e 7 dell'OSI (Sessione, Presentazione, Applicazione).
2. **Transport (Trasporto):** corrisponde al livello 4 OSI (TCP, UDP).
3. **Internet:** corrisponde al livello 3 OSI (IP).
4. **Network Access / Host-to-Network:** accorpa i livelli 1 e 2 OSI (Fisico e Data Link).

| Livello | Modello ISO/OSI (7 livelli) | Stack TCP/IP (4 livelli) | Modello Ibrido (5 livelli) | Protocolli / Concetti chiave |
| :---: | :--- | :--- | :--- | :--- |
| **7** | **Application** (Applicazione) | **Application** (Applicazione) | **Application** | HTTP, DNS, SMTP, FTP, SSH |
| **6** | **Presentation** (Presentazione) | *(accorpato in Application)* | *(accorpato in Application)* | Codifica, cifratura, compressione |
| **5** | **Session** (Sessione) | *(accorpato in Application)* | *(accorpato in Application)* | Gestione dialogo e sincronizzazione |
| **4** | **Transport** (Trasporto) | **Transport** (Trasporto) | **Transport** | TCP, UDP |
| **3** | **Network** (Rete) | **Internet** | **Network / Internet** | IP, ICMP, ARP, routing |
| **2** | **Data Link** (Collegamento) | **Network Access** *(Host-to-Network)* | **Data Link** | Ethernet (802.3), Wi-Fi (802.11), MAC, Switch |
| **1** | **Physical** (Fisico) | *(accorpato in Network Access)* | **Physical** | Cavi, fibra ottica, segnali radio, bit |

---

## 4. Standardizzazione nelle Telecomunicazioni
* **Standard *de facto*:** tecnologie o protocolli affermati spontaneamente sul mercato grazie alla diffusione commerciale (es. Ethernet nelle prime fasi, PDF, stack TCP/IP).
* **Standard *de jure*:** norme approvate formalmente da organismi internazionali riconosciuti.

### Principali Enti di Standardizzazione
* **[[ISO]]** (*International Organization for Standardization*): organismo internazionale che ha definito il modello OSI.
* **[[IEEE]]** (*Institute of Electrical and Electronics Engineers*): definisce gli standard per reti locali e metropolitane (famiglia **IEEE 802**, come 802.3 Ethernet, 802.5 Token Ring, 802.11 Wi-Fi).
* **[[ITU]]** (*International Telecommunication Union*): agenzia ONU per le telecomunicazioni e le radiocomunicazioni (ex CCITT).
* **[[IETF]]** (*Internet Engineering Task Force*): sviluppa gli standard tecnici di Internet tramite i documenti **[[RFC]]** (*Request for Comments*).
* **[[W3C]]** (*World Wide Web Consortium*): fondato da Tim Berners-Lee, cura gli standard del Web (HTML, CSS, XML).
* **[[ICANN]]:** coordina l'assegnazione globale di indirizzi IP e nomi a dominio.