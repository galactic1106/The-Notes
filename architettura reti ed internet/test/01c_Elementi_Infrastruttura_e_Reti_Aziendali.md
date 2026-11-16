---
corso: Architettura Reti ed Internet
modulo: Modulo I - Prof. Osvaldo Gervasi
fonte: ArchitetturaRetiDef_I_a.pdf
argomento: Elementi dell'infrastruttura di Internet, Intranet, Extranet e DMZ
tags:
  - reti
  - infrastruttura
  - isp
  - intranet
  - extranet
  - dmz
---

# 🏗️ Elementi dell'Infrastruttura di Internet e Reti Aziendali

## 1. Gerarchia dell'Infrastruttura di Internet
L'infrastruttura globale collega gli utenti finali ai fornitori di contenuti attraverso quattro stadi principali:

1. **User Location (Sede Utente):**
   * **Terminali multimediali (PC/Mobile):** dotati di sottosistemi audio, video e moduli di sicurezza/cifratura (*Encryption*).
   * **CPE (*Customer Premise Equipment*):** apparati presso l'utente (modem/router con funzioni integrate di **[[NAT]]**, **[[DHCP]]**, **[[Firewall]]** e *Port Forwarding*) e rete locale cablata (**[[Ethernet]]**) o wireless (**[[Wi-Fi]]**).
2. **Local Loop Carrier (Rete di Accesso / "Ultimo Miglio"):**
   * Infrastruttura che collega la sede utente alla centrale locale dell'operatore tramite doppino in rame (**[[ADSL]]**/xDSL, linee dedicate), fibra ottica (**[[FTTH]]**), reti via cavo, ponti radio/wireless, satellite o onde convogliate su rete elettrica (*Power Line*).
3. **ISP (*Internet Service Provider*) e POP:**
   * **POP (*Point of Presence*):** punto di accesso fisico all'ISP dove confluiscono le linee provenienti dal *Local Loop*.
   * **User Services:** server ad alta affidabilità (*fault tolerant* e *load balanced*) per **[[DNS]]** (es. BIND), posta elettronica (Sendmail, Postfix), accesso remoto ([[SSH]]) e hosting.
4. **ISP Backbone (Dorsali) e Interconnessione:**
   * Dorsali ad altissima capacità in fibra ottica con router ad alte prestazioni, switch **SONET/SDH** e **Gigaswitch**.
   * Gli ISP si interconnettono tramite accordi diretti (**Private Peering**) o punti di scambio neutri chiamati **[[NAP]]** (*Neutral Access Point*) o **[[IXP]]** (*Internet Exchange Point*).
   * **Gerarchia Tier:**
     * **Tier 1:** dorsali globali interconnesse via peering gratuito senza pagare transito.
     * **Tier 2:** operatori nazionali/regionali che acquistano transito dai Tier 1 e fanno peering agli IXP.
     * **Tier 3:** ISP locali che forniscono accesso diretto agli utenti finali (*single-homed* o *multi-homed*).
5. **Online Content e [[Cloud Computing]]:**
   * *Web Server Platforms* e *Hosting Farms* che ospitano servizi web, audio e video; le architetture **Cloud** garantiscono scalabilità, alta affidabilità e costi sostenibili.

---

## 2. Architetture di Rete Aziendali: Intranet, Extranet e DMZ

* **[[Intranet]]:** rete privata interna a un'organizzazione che utilizza i protocolli standard di Internet (**TCP/IP**, HTTP, DNS, SMTP) per condividere informazioni, documenti e applicativi esclusivamente tra i dipendenti interni.
* **[[Extranet]]:** estensione controllata e sicura della Intranet verso soggetti esterni selezionati (fornitori, partner commerciali, clienti), realizzata tramite canali autenticati e cifrati (es. **[[VPN]]**).
* **[[DMZ]] (*Demilitarized Zone* / Zona Demilitarizzata):** sottorete perimetrale isolata posta tra la rete interna (Intranet) e la rete esterna non fidata (Internet), protetta da uno o due **[[Firewall]]** (architettura *dual-homed* o *screened subnet*).
  * **Scopo:** ospitare i server che devono essere raggiungibili dall'esterno (Web server pubblici, Mail server, DNS esterni, Proxy server) senza esporre direttamente la rete interna aziendale; in caso di compromissione di un server nella DMZ, il firewall interno impedisce all'attaccante di accedere alla Intranet.