---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_e.pdf
argomento: Metodi di accesso deterministico al mezzo, IEEE 802.4 Token Bus e IEEE 802.5 Token Ring
tags:
  - reti
  - token-ring
  - token-bus
  - ieee-802-4
  - ieee-802-5
  - data-link
---

# 🔄 Reti a Passaggio di Gettone: Token Bus e Token Ring

## 1. Accesso Deterministico vs Accesso Casuale
A differenza di Ethernet (basato sulla contesa probabilistica del mezzo con CSMA/CD), le reti a **passaggio di gettone (*token passing*)** adottano un controllo dell'accesso **deterministico** e **privo di collisioni**:

* Una trama speciale di controllo chiamata **Token (gettone)** circola continuamente tra le stazioni secondo un ordine logico predefinito.
* Solo la stazione che possiede il token ha il diritto esclusivo di trasmettere dati sul mezzo per un tempo massimo prefissato (**THT**, *Token Holding Time*).
* **Vantaggi:** assenza totale di collisioni, comportamento deterministico con ritardo massimo garantito (*bounded delay*, ideale in ambito industriale e real-time) e supporto nativo alle priorità.
* **Svantaggi:** maggiore complessità protocollaria (gestione della perdita o duplicazione del token, ingresso/uscita dinamica dei nodi) e overhead anche a basso carico.

---

## 2. [[Token Bus]] (Standard IEEE 802.4)
Combina la topologia **fisica a bus** (tipicamente su cavo coassiale a banda larga) con una **topologia logica ad anello**:

* Ogni stazione conosce l'indirizzo del proprio **predecessore** e del proprio **successore** nell'anello logico.
* Il token viene passato esplicitamente da una stazione alla successiva inviando una trama indirizzata al successore sul bus condiviso.
* **Manutenzione dell'anello:** esistono procedure specifiche per l'aggiunta di nuove stazioni (*Solicit Successor*), l'uscita ordinata di un nodo (*Set Successor*) e la rigenerazione del token in caso di guasto.
* **Ambito applicativo:** automazione industriale (standard **MAP**, *Manufacturing Automation Protocol*).

---

## 3. [[Token Ring]] (Standard IEEE 802.5)
Sviluppato originariamente da IBM (e standardizzato come **IEEE 802.5**), utilizza una topologia **fisica a stella** cablata tramite concentratori passivi/attivi detti **[[MAU]]** (*Multi-Station Access Unit*), che realizzano internamente un **anello logico unidirezionale**:

### Meccanismo di Funzionamento
1. **Connessione tramite MAU:** ogni stazione è collegata a una porta della MAU tramite connettori IBM Type 1 o RJ-45. La MAU contiene relè che bypassano automaticamente una stazione spenta o guasta, preservando la continuità elettrica dell'anello.
2. **Trasmissione della trama:**
   * Quando una stazione vuole trasmettere, attende il passaggio del **token libero** (3 byte: `SD`, `AC`, `ED`).
   * Cattura il token modificando il bit *Token* nel campo *Access Control* (`AC`), trasformandolo nell'intestazione di una **trama dati**, e accoda il payload e l'indirizzo del destinatario.
   * La trama percorre l'intero anello di stazione in stazione (ciascuna agisce da ripetitore attivo con ritardo di $1\text{ bit}$).
   * Il **destinatario** copia la trama nei propri buffer e imposta i bit di stato (**A** = *Address Recognized* e **C** = *Frame Copied*) nel campo **FS** (*Frame Status*) in coda alla trama.
   * La trama torna al **mittente**, che verifica i bit `A` e `C` come conferma di ricezione, rimuove la trama dall'anello (*stripping*) e rilascia un nuovo token libero.
3. **Early Token Release:** nelle versioni a $16\text{ Mbps}$, il mittente può rilasciare il token subito dopo aver trasmesso l'ultimo bit della trama senza attendere il giro completo sull'anello, migliorando l'efficienza.
4. **Active Monitor:** una stazione designata sull'anello vigila sul corretto funzionamento, rimuove trame orfane che circolano all'infinito e rigenera il token in caso di smarrimento.

### Confronto Sintetico: Ethernet (802.3) vs Token Ring (802.5)

| Caratteristica | Ethernet (IEEE 802.3) | Token Ring (IEEE 802.5) |
| :--- | :--- | :--- |
| **Accesso al mezzo** | Casuale a contesa (CSMA/CD) | Deterministico a passaggio di token |
| **Collisioni** | Possibili su mezzo condiviso | Assenti |
| **Prestazioni a basso carico** | Ottime (trasmissione immediata) | Lieve latenza di attesa del token |
| **Prestazioni ad alto carico** | Degradano per l'aumento delle collisioni | Stabili e prevedibili |
| **Complessità hardware/protocollo** | Bassa ed economica | Elevata |