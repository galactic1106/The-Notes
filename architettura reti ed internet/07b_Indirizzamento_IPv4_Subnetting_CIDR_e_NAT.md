---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_g.pdf
argomento: Indirizzamento IPv4, classi storiche, Subnetting, CIDR, indirizzi privati e NAT
tags:
  - reti
  - ipv4
  - subnetting
  - cidr
  - nat
  - indirizzamento
---

# 🔢 Indirizzamento IPv4, Subnetting, CIDR e NAT

## 1. Struttura dell'Indirizzo [[IPv4]]
Un indirizzo IPv4 è un numero binario a **$32\text{ bit}$** ($4\text{ byte}$), rappresentato comunemente in **notazione decimale puntata** (*dotted-decimal*, quattro ottetti compresi tra $0$ e $255$, es. `192.168.1.10`). Esso si compone di due parti gerarchiche:
* **Network ID (Prefisso di rete):** identifica la sottorete logica a cui appartiene l'host.
* **Host ID:** identifica univocamente la specifica interfaccia all'interno di quella sottorete.

### Indirizzi Speciali
* **Indirizzo di Rete:** tutti i bit dell'*Host ID* posti a `0` (identifica l'intera sottorete, non assegnabile a un host).
* **Indirizzo di Broadcast Diretto:** tutti i bit dell'*Host ID* posti a `1` (invia pacchetti a tutti gli host della sottorete).
* **Limited Broadcast (`255.255.255.255`):** broadcast limitato alla rete locale corrente (non inoltrato dai router).
* **Loopback (`127.0.0.0/8`, tipicamente `127.0.0.1`):** usato per comunicazioni interne sullo stesso host (*localhost*).

---

## 2. Indirizzamento Classful (Storico) vs [[CIDR]]
Originariamente lo spazio IPv4 era suddiviso in **5 classi fisse** (*Classful Addressing*) in base ai primi bit dell'indirizzo:

| Classe | Primi Bit | Range Primo Ottetto | Maschera Default | Reti e Host Disponibili |
| :---: | :---: | :---: | :---: | :--- |
| **A** | `0` | $1 - 126$ | `255.0.0.0` (`/8`) | $126$ reti da $2^{24}-2 = 16.777.214$ host |
| **B** | `10` | $128 - 191$ | `255.255.0.0` (`/16`) | $16.384$ reti da $2^{16}-2 = 65.534$ host |
| **C** | `110` | $192 - 223$ | `255.255.255.0` (`/24`) | $2.097.152$ reti da $2^{8}-2 = 254$ host |
| **D** | `1110` | $224 - 239$ | — | Riservata al traffico **Multicast** |
| **E** | `1111` | $240 - 255$ | — | Riservata per usi sperimentali |

### Superamento delle Classi: [[Subnetting]] e [[CIDR]]
La rigidità delle classi causava un enorme spreco di indirizzi (es. un'azienda con $500$ host doveva richiedere una Classe B da oltre $65.000$ indirizzi) e la crescita esplosiva delle tabelle di routing. Si sono introdotte quindi due tecniche fondamentali:

1. **Subnetting e [[Subnet Mask]]:**
   * La **Subnet Mask** è una sequenza di $32\text{ bit}$ composta da tutti `1` in corrispondenza del *Network ID* + *Subnet ID* e tutti `0` in corrispondenza dell'*Host ID*.
   * Eseguendo l'operazione logica **AND bit a bit** tra l'indirizzo IP e la Subnet Mask si ottiene l'indirizzo della sottorete di appartenenza.
   * Presi $s$ bit dall'*Host ID* per creare sottoreti, si ottengono $2^s$ sottoreti, ciascuna con $2^h - 2$ host utilizzabili (dove $h$ sono i bit rimanenti per l'host).
2. **CIDR (*Classless Inter-Domain Routing* – RFC 1519):**
   * Elimina completamente le classi fisse usando la **notazione con prefisso** (`a.b.c.d/x`, dove `/x` indica il numero di bit a `1` della maschera).
   * Permette sia il **VLSM** (*Variable Length Subnet Mask*, sottoreti di dimensioni diverse calibrate sulle reali esigenze) sia il **Route Aggregation / Supernetting** (accorpamento di più reti contigue in un'unica voce nella tabella di routing dei router di dorsale).

---

## 3. Indirizzi IP Privati (RFC 1918) e [[NAT]]
Per arginare l'esaurimento degli indirizzi IPv4 pubblici, la **RFC 1918** ha riservato tre blocchi di **indirizzi privati** non instradabili sull'Internet pubblica (riutilizzabili liberamente in ogni rete locale):

* **Classe A privata:** `10.0.0.0/8` (`10.0.0.0` – `10.255.255.255`)
* **Classe B privata:** `172.16.0.0/12` (`172.16.0.0` – `172.31.255.255`)
* **Classe C privata:** `192.168.0.0/16` (`192.168.0.0` – `192.168.255.255`)

### Funzionamento di [[NAT]] e [[PAT]] (*Port Address Translation*)
Affinché gli host con indirizzo privato possano comunicare su Internet, il router di confine esegue la traduzione **NAT / PAT (NAPT)**:
1. **In uscita:** il router sostituisce l'indirizzo IP sorgente privato (e la porta sorgente TCP/UDP) con il proprio **indirizzo IP pubblico** e un numero di porta univoco, salvando l'associazione nella **NAT Translation Table**.
2. **In ingresso:** quando arriva la risposta dal server remoto verso quell'IP pubblico e quella specifica porta, il router consulta la tabella NAT, ripristina l'IP privato e la porta dell'host interno e inoltra il pacchetto nella LAN.