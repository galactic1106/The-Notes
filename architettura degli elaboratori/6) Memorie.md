- Registri
- Micro operazioni
- 

ci troviamo sul livello RTL ( Register Transfer Language ) cioè il livello funzionale.
Contiene:
- L'insieme dei registri e degli altri moduli elementari con le rispettive funzioni
- Le sequenze di micro operazioni che si possono eseguire sulle informazioni presenti nei registri
- La funzione di controllo che inizializza tali sequenze

Una formula classica è $P : A \leftarrow B$ che copia il valore del registro $B$ nel registro $A$.

## Opp Code
| Simbolo                                                          | Descrizione                                           | Esempio          |
| ---------------------------------------------------------------- | ----------------------------------------------------- | ---------------- |
| \<Maiuscola\>                    <br>{\<Maiuscola\>\|\<numero\>} | Registro                                              | A, B<br>A3, MBR  |
| \<pedice\>                                                       | Particolare bit                                       | $A_2, B$         |
| \<parentesi\>                                                    | Porzione Registro                                     | $A(1-5), A(add)  |
| \<freccia\>                                                      | Trasferimento                                         | $A \leftarrow B$ |
| \<due punti\>                                                    | Indicano la terminazione di una funzione di controllo | $C: i f(x,y)$    |
| \<virgola\>                                                      | separazione tra 2 micro funzioni di controllo         |                  |


![[MOV operation PDP]]
![[Shift Operation PDP8]]
![[Transfer Parallelo]]

Se voglio che $n$ registri parlino tra di loro allora ho bisogno di $n(n-1)/2$ Collegamenti

