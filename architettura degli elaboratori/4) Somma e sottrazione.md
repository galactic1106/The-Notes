Si considera la codifica in complemento a 2 $\implies$ che abbiamo bisogno di sapere la dimensione delle stringe con cui lavoriamo

Se si fa la somma e il risultato **sovrascrive con i riporti il bit significativo $\iff$ cambia il segno** allora bisogna inviare un warning.


| Operazione | A     | B     | Risultato |
| ---------- | ----- | ----- | --------- |
| A+B        | $>=0$ | $>=0$ | $<0$      |
| A+B        | $<0$  | $<0$  | >=0       |
| A-B        | $>=0$ | $<0$  | <0        |
| A-B        | $<0$  | $>=0$ | $>=0$     |

