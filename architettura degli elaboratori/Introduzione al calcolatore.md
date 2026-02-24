## Cos'è un calcolatore?
un calcolatore è una macchina in grado di eseguire calcoli in modo automatico:
- prende un ingresso informazioni e dati
- elabora i dati secondo regole stabilite da una lista predefinita di istruzioni macchina
- manda in uscita i risultati dell'elaboratore sotto forma di informazioni / dati
  
## Architettura di un elaboratore
L'architettura di un elaboratore, è in più generale un sistema di elaborazione, è costituita da due componenti fondamentali:
- Hardware (insieme delle componenti fisiche ed elettroniche)
- Software (insieme dei programmi)
- Firmware

descrive le caratteristiche visibili all' utente, cioè quello che influenza direttamente la scrittura dei programmi, è quindi noto al programmatore:
- set di istruzioni (ISA) 
- Registri disponibili
- Modalità di indirizzamento
- Formato dei dati (32/64 bit)
- Spazio di memoria indirizzabile
In breve definisce il modello astratto della macchina


## Organizzazione di un calcolatore

Riguarda le relazioni strutturali tra le unità fondamentali e il modo in cui esse realizzano una data architettura
tendenzialmente non è visibile al programmatore:
- la tecnologia impiegata
- frequenza di clock
- modalità di esecuzione di un istruzione

Decidere se la moltiplicazione farà parte del set di istruzioni e se avrà un unità dedicata per essere eseguita o si appoggerà alla somma sono entrambe quesiti architetturali.

### Esempio: Mouse 
L'architettura riguarda:
- Il numero di pulsanti
- Presenza di una rotellina
- Tipi di segnali
- Interfaccia fisica ( USB / PS2 )
  
## Rappresentazione per Astrazione
- Nelle scienze la realtà è rappresentata attraverso modelli
- Poiché un calcolatore costituisce un sistema estremamente complesso, risulta conveniente

### Livelli di astrazione
- Livello delle applicazioni: 
	- Strutture: programmi applicativi
	- Componenti: sistema operativo, librerie, file system
	
- Livello di linguaggio macchina:
	- Strutture: programmi
	- Componenti: modello di programmazione, repertorio istruzioni
	
- Libello funzionale (RTL => register transfer language):
	- Strutture: unità di controllo, modello di programmazione
	- Componenti: registri, bus, memorie
	
- Libello della logica:
	- Strutture: registri, contatori,unità aritmetiche, memorie
	- Componenti: porte, flip-flop, dock
	
- Livello dei circuiti
	- Strutture: porte logiche, flip-flop, drive
	- Componenti: transistori, resistenze, capacità

## Codifiche:
- Binaria
- BCD (Binary Code Decimal)
- ASCII
- EBCDIC (Extended Binary Code Decimal Interchange Code)
- Standard IEEE 754 (codifica dei numeri)