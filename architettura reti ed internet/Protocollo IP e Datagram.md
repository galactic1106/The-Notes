---
corso: Architettura Reti ed Internet
università: Università degli Studi di Perugia (UNIPG)
argomento: Protocollo IPv4, formato del datagram e campi dell'header
tags:
  - reti
  - ip
  - ipv4
  - datagram
  - header-ip
  - routing
---

# 📦 Protocollo IP e Formato del Datagram (IPv4)

## 1. Funzioni Fondamentali dell'[[IP]] (*Internet Protocol*)
* Definisce l'esatto formato dei dati mentre attraversano l'internet [[TCP-IP]].
* Svolge la funzione di **routing**, scegliendo il percorso che i dati devono seguire.
* Stabilisce l'insieme di regole per:
  * Consegna **non affidabile** (*best-effort*) dei pacchetti.
  * Elaborazione dei pacchetti da parte di host e gateway (router).
  * Generazione dei messaggi di errore tramite **[[ICMP]]**.
  * Determinazione delle condizioni in cui occorre scartare i pacchetti.

---

## 2. Il Datagram IPv4
L'unità fondamentale di trasferimento del protocollo IP è il **datagram IP**, suddiviso in:
1. **Header** (intestazione, minimo 20 byte senza opzioni).
2. **Data** (blocco dei dati trasportati).

### Struttura dell'Header IPv4 (parole da 32 bit)
```text
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|      Fragment Offset    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |         Header Checksum       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source IP Address                       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination IP Address                     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options (if any) + Padding                 |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

```

