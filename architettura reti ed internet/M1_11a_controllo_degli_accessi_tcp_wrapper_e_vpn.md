---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_m.pdf
argomento: Controllo degli accessi (AAA), TCP Wrapper e Virtual Private Network (VPN, IPsec)
tags:
  - reti
  - sicurezza
  - controllo-accessi
  - tcp-wrapper
  - vpn
  - ipsec
---

# 🔑 Controllo degli Accessi, TCP Wrapper e VPN

## 1. Modello AAA (*Authentication, Authorization, Accounting*)
La gestione sicura degli accessi alle risorse di rete si articola su tre livelli logici:
1. **Authentication (Autenticazione):** verifica dell'identità dell'utente o del dispositivo che richiede l'accesso (tramite password, certificati digitali, token hardware o biometria — *autenticazione a più fattori / MFA*).
2. **Authorization (Autorizzazione):** determinazione dei privilegi e delle operazioni consentite all'utente già autenticato (es. permessi di lettura/scrittura, accesso a specifiche sottoreti o servizi).
3. **Accounting (Tracciamento / Auditing):** registrazione nei file di log (*logging*) delle attività svolte dall'utente (orari di connessione, risorse utilizzate, volume di traffico) per finalità di analisi, sicurezza e conformità.

---

## 2. [[TCP Wrapper]] e Limitazione degli Accessi Host-Based
Il **TCP Wrapper** (sviluppato da Wietse Venema) è un meccanismo di controllo degli accessi a livello host (*host-based ACL*) utilizzato nei sistemi Unix/Linux per filtrare le connessioni in ingresso dirette ai demoni di rete (come `sshd`, `ftpd`, `telnetd`, `portmap`) compilati con la libreria `libwrap` o avviati tramite super-server (`inetd` / `xinetd`).

### File di Configurazione e Regole di Valutazione
Il controllo si basa su due file testuali situati in `/etc/`:
1. **`/etc/hosts.allow`:** elenca gli host o le reti autorizzate ad accedere a determinati servizi.
2. **`/etc/hosts.deny`:** elenca gli host o le reti a cui l'accesso è negato.

* **Sintassi delle regole:**
  ```text
  lista_demoni : lista_client [: comando_opzionale]
  ```
  *Esempio in `/etc/hosts.allow`:* `sshd : 192.168.1.0/255.255.255.0, .unipg.it`
  *Esempio in `/etc/hosts.deny`:* `ALL : ALL` (politica *default deny*).
* **Ordine di elaborazione:**
  1. Il sistema controlla prima **`/etc/hosts.allow`**: se trova una corrispondenza, l'accesso viene **immediatamente consentito** e la ricerca termina.
  2. Se non trova corrispondenze, controlla **`/etc/hosts.deny`**: se trova una corrispondenza, l'accesso viene **negato**.
  3. Se non vi è alcuna corrispondenza in nessuno dei due file, l'accesso viene **consentito per default**.

> [!tip] Difesa in profondità
> Il TCP Wrapper opera a livello applicativo/socket e affianca il firewall di rete (`iptables`/`nftables`), permettendo inoltre di eseguire azioni personalizzate al rilevamento di un tentativo non autorizzato (es. invio di alert o logging esteso).

---

## 3. [[VPN]] (*Virtual Private Network*)
Una **VPN** crea un **tunnel logico cifrato** attraverso una rete pubblica non fidata (come Internet), estendendo una rete privata in modo sicuro ed economico rispetto alle linee dedicate fisiche (*leased lines*).

### Tipologie Architetturali di VPN
* **Site-to-Site VPN (LAN-to-LAN):** collega permanentemente due o più sedi aziendali geograficamente distanti tramite gateway VPN dedicati (router o firewall); gli host interni comunicano in modo trasparente senza dover installare client VPN specifici.
* **Remote Access VPN (Host-to-LAN):** consente a singoli utenti mobili (*teleworkers*) di collegarsi in sicurezza alla rete aziendale tramite un software client installato sul proprio dispositivo.

### Protocolli Principali per VPN
* **[[IPsec]] (*Internet Protocol Security*):** suite di protocolli standard (IETF) che opera al **Livello 3 (Network)** proteggendo direttamente i pacchetti IP:
  * **AH (*Authentication Header*):** garantisce autenticazione dell'origine e integrità dei dati, ma **non cifra** il payload.
  * **ESP (*Encapsulating Security Payload*):** garantisce **riservatezza (cifratura)**, integrità e autenticazione.
  * **Modalità operative:**
    * *Transport Mode:* cifra solo il payload del pacchetto IP lasciando intatto l'header IP originale (usata per comunicazioni end-to-end tra due host).
    * *Tunnel Mode:* incapsula e cifra l'**intero pacchetto IP originale** all'interno di un nuovo datagramma IP con nuovo header (modalità standard per le VPN *Site-to-Site* tra gateway).
  * **IKE (*Internet Key Exchange*):** protocollo che negozia automaticamente le associazioni di sicurezza (**SA – *Security Association***) e le chiavi crittografiche di sessione.
* **SSL/TLS VPN (es. OpenVPN) e WireGuard:** operano a livello Trasporto/Applicazione (spesso su porte TCP/UDP standard come la 443 o la 1194), attraversando facilmente NAT e firewall senza richiedere configurazioni complesse lato client.