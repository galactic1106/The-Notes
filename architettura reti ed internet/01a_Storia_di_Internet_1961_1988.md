---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_a.pdf
argomento: Storia di Internet dalle origini (1961) al 1988
tags:
  - reti
  - storia-internet
  - arpanet
  - tcp-ip
  - nsfnet
---

# 🌐 Storia di Internet (1961 – 1988)

## 1. L'Impatto di Internet e il "Villaggio Globale"
* **Rivoluzione tecnologica e culturale:** Internet è la principale rivoluzione tecnologica del XX secolo e il più rivoluzionario mezzo di comunicazione umana; distrugge il tempo e lo spazio, abolisce le frontiere e crea una rete di reti ("autostrade elettroniche" che collegano isole di connettività).
* **Villaggio globale:**
  * **Ricerca scientifica e didattica:** sviluppo delle reti della ricerca (es. rete **GARR** in Italia), posta elettronica, videoconferenze, accesso a risorse remote (**SSH**, *Remote Job Entry*), **HPC**, *Machine Learning*, *Cybersecurity*, laboratori virtuali e didattica/valutazione online.
  * **Aziende:** marketing, branding, social networking, rapporti con clienti e rete di vendita, **Intranet** ed **e-commerce**.
  * **Pubbliche Amministrazioni:** Rete delle Pubbliche Amministrazioni (ex AIPA, DigitPA, **AgID**), Codice dell'Amministrazione Digitale (**CAD**), Agenda Digitale, dematerializzazione, riuso, adozione di software libero/open source (**FLOSS**) e *disaster recovery*.

---

## 2. Le Fondamenta Teoriche e i Primi Esperimenti (1961 – 1968)
* **1961 – 1964 (Leonard Kleinrock, MIT):** pubblica il primo articolo sulla teoria della **commutazione di pacchetto (*Packet Switching*)** (*"Information Flow in Large Communication Nets"*, luglio 1961) e il primo libro in materia (*"Communication Nets: Stochastic Message Flow and Delay"*, 1964).
* **Agosto 1962 (J.C.R. Licklider & W. Clark, MIT):** introducono il concetto di ***Galactic Network*** (*"On-Line Man Computer Communication"*), immaginando un insieme di computer globalmente interconnessi per scambiare dati, informazioni e programmi.
* **Anni '50/'60 – Sistema SAGE (*Semi Automatic Ground Environment*):** sviluppato da MIT e IBM, fu il primo sistema di difesa aerea del Nord America basato su puntamento ottico e schermi radar; gettò le basi per i sistemi di prenotazione viaggi e controllo del traffico aereo.
* **1965 (Lawrence Roberts & Thomas Merrill):** collegano il computer **TX-2** al MIT (Massachusetts) con il **Q-32** (California) tramite linea telefonica *dial-up* a bassa velocità, creando la prima *Wide-Area Computer Network*. Si dimostra che i computer *time-shared* possono cooperare, ma le linee telefoniche a commutazione di circuito sono inadeguate, confermando la necessità del *Packet Switching*.
* **Agosto 1968:** viene completata la specifica tecnica di **ARPANET** (su impulso dell'agenzia ARPA del Dipartimento della Difesa USA).

> [!quote] Aneddoto storico – Vint Cerf (anni '60)
> *"Jon drew all the pictures, Steve and I strung the zucchini and the yellow squash. I think we must have collectively spent about 8 hours on this. Note that this network can't work - there is no mouth/ear link anywhere!!! Such was the state of networking in the primitive 1960s..."* — **Vint Cerf**

---

## 3. Nascita di ARPANET e Sviluppo dei Protocolli (1969 – 1982)
* **7 aprile 1969 (Steve Crocker):** pubblica la prima **RFC (*Request for Comments*)**, avviando la documentazione aperta e collaborativa dell'architettura di ARPANET e di Internet.
* **25 ottobre 1969 – Primi 2 nodi ARPANET:** prima connessione *host-to-host* tra il *Network Measurement Center* di **UCLA** (nodo 1) e lo **Stanford Research Institute (SRI)** (nodo 2) tramite gli **IMP** (*Interface Message Processor*). Al primo tentativo di login (`LO`) il sistema va in crash, al secondo ha successo. Entro fine 1969 si aggiungono **UCSB** e l'**Università dello Utah** (4 nodi totali).
* **1971 – 1972:**
  * Nasce il protocollo **NCP** (*Network Control Protocol*).
  * **Ray Tomlinson** (BBN) inventa la **posta elettronica (email)** e introduce il simbolo `@` per separare utente e macchina.
  * Prima dimostrazione pubblica di ARPANET alla conferenza ICCC (40 macchine collegate).
* **1973 – 1974 (Bob Kahn & Vint Cerf):** progettano un'architettura per l'*internetworking* (interconnessione di reti eterogenee) e pubblicano la specifica del protocollo **TCP** (*Transmission Control Protocol*), che verrà poi separato in **TCP** (trasporto affidabile) e **IP** (indirizzamento e instradamento).
* **1976 (Robert Metcalfe, Xerox PARC):** inventa **Ethernet** per le reti locali (LAN) su cavo coassiale.
* **1978:** Koji Kobayashi (NEC) pubblica al MIT *"Computers & Communications"* (**C&C**), delineando la convergenza tra informatica e telecomunicazioni.

---

## 4. Transizione a TCP/IP, DNS ed Espansione Globale (1983 – 1988)
* **1° gennaio 1983 (*Flag Day*):** ARPANET abbandona definitivamente NCP e adotta ufficialmente la suite **TCP/IP** (RFC 801).
* **Novembre 1983 (Jon Postel, Paul Mockapetris & Craig Partridge):** sviluppano il **[[DNS]] (*Domain Name System*)**, introducendo i nomi a dominio e l'indirizzamento `user@host.domain`.
* **1983 – 1984:**
  * **Sun Microsystems** (fondata da Bill Joy) lancia workstation con TCP/IP integrato; **Apollo** introduce workstation con rete **Token Ring**; **Apple** presenta il **Macintosh** (gennaio 1984).
  * Vengono introdotte le reti IP di **classe A, B e C** e i primi **Top-Level Domain (TLD)** generici (`.edu`, `.com`, `.net`, `.org`) e nazionali ISO (`.it`, `.fr`, `.de`, ecc.).
  * William Gibson conia il termine *cyberspace* nel romanzo *Neuromancer*.
  * Nasce nel Regno Unito la rete accademica **JANET** e negli USA la **NSF** (*National Science Foundation*) istituisce i centri di supercalcolo.
  * Il modello **ISO/OSI** mostra scarsa adozione pratica rispetto alla rapidità di diffusione di TCP/IP.
* **1985 – 1986:**
  * L'algoritmo di routing unico viene sostituito da protocolli gerarchici: **IGP** (*Interior Gateway Protocol*, interno alle organizzazioni) ed **EGP** (*Exterior Gateway Protocol*, tra reti diverse).
  * Nasce il backbone **NSFNET** a 56 kbps basato su TCP/IP.
  * Gli host passano da 2.000 a 30.000; Ethernet si afferma nei campus e nascono aziende chiave del networking (Cisco, IBM, Proteon, SynOptics, Wellfleet, Cabletron).
  * **30 aprile 1986:** **l'Italia si connette per la prima volta ad Internet** (dal CNUCE di Pisa verso gli Stati Uniti).
* **1987 – 1988:**
  * Gli host salgono a 80.000 (gennaio 1988), 130.000 (luglio) e 160.000 (novembre).
  * Si connettono numerosi Paesi (Italia, Germania, Inghilterra, Olanda, Israele, Giappone, Australia, Nuova Zelanda, Messico).
  * La dorsale NSFNET passa alle linee **T-1** (1,544 Mbps), diventano operative le linee **T-3** (45 Mbps), compaiono connessioni **FDDI** a 100 Mbps su fibra e il protocollo **SMDS**.