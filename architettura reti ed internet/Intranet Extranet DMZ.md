---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
argomento: Reti aziendali, Intranet, Extranet e DMZ
tags:
  - reti
  - intranet
  - extranet
  - sicurezza
  - vpn
  - dmz
---

# 🏢 Reti Aziendali: Intranet, Extranet e DMZ

L'adozione delle tecnologie Internet in ambito aziendale permette di collegare sedi interne, integrare sistemi preesistenti ed esporre servizi verso l'esterno in modo controllato.

---

## 1. [[Intranet]]
Una **Intranet** è una rete privata aziendale che utilizza i protocolli e i servizi tipici di Internet (Web, posta, ecc.) per la comunicazione e la condivisione interna delle risorse.

### Vantaggi organizzativi e tecnologici
* **Diffusione delle informazioni:** le tecnologie telematiche e i servizi di rete si diffondono **orizzontalmente** (tra reparti allo stesso livello) e **verticalmente** (tra diversi livelli gerarchici) nella struttura aziendale.
* **Crescita interna:** diventa uno strumento fondamentale di **partecipazione**, **formazione** e **aggiornamento** continuo del personale.
* **Versatilità del terminale:** avendo il computer come periferica di accesso, sfrutta la sua capacità di trasmettere, integrare e rappresentare qualsiasi tipologia di informazione multimediale.

### Interconnessione tra più sedi aziendali
Quando un'organizzazione dispone di più sedi geograficamente distanti (es. *LAN sede A*, *LAN sede B*, *LAN sede C*):
* Ogni sede locale è protetta dal proprio **[[Firewall]]** e collegata alla rete pubblica tramite un **[[Router]]**.
* Le diverse LAN aziendali comunicano attraverso **Internet** creando tunnel cifrati tramite **[[VPN]]** (*Virtual Private Network*), garantendo riservatezza e integrità dei dati come su una rete privata dedicata.

---

## 2. [[Extranet]]
Con il termine **Extranet** si identificano le risorse hardware e software che realizzano la **presenza visibile su Internet** di un'organizzazione, estendendo l'accesso a determinati servizi anche a utenti esterni (clienti, partner, fornitori).

### Servizi tipici
* **Servizi Web** pubblici o ad accesso riservato.
* **E-commerce** (commercio elettronico).
* **Data Warehouse** e **Data Mining** (accesso controllato a basi di dati aziendali).

---

## 3. [[DMZ]] (*De-Militarized Zone*)
Per proteggere la rete interna aziendale (LAN) quando si espongono servizi Extranet verso Internet, si utilizza un'architettura segmentata tramite **[[Firewall]]**:

* **Posizionamento:** i server che erogano servizi visibili dall'esterno (es. server **HTTP**, **LDAP**, **J2EE**) vengono collocati in una sottorete isolata detta **DMZ** (*Zona Demilitarizzata*), posta tra il *Gateway* (verso Internet) e la *LAN* interna.
* **Politiche di sicurezza:** nella DMZ il controllo del firewall è più lasco rispetto alla rete interna per consentire le connessioni provenienti da Internet.
* **Gestione dei dati critici:** i server presenti nella DMZ **non sono ritenuti critici**; i servizi e i dati esposti sono generalmente **replicati da server protetti** situati all'interno della LAN aziendale, riducendo il rischio in caso di compromissione dall'esterno.