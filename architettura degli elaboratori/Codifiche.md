## Notazione posizionale

- Si fissa una base B
- Si hanno a disposizione B
- Il valore di una cifra dipende dalla posizione che occupa

### Conversione di base generale
$$ 
(b_{n-1}. b_{n-2}, ..., b_{1}, b_{0} )_{B}= 
b_{0}B^{0} + b_{1}B^{1} + \dots + b_{n-1}B^{n-1} + b_{n}B^{n}= 
\sum_{i=0}^{n-1} b_{i}B^{i} 
$$
#### $base 10 \to base 2$
$$
4130_{10} \to base 2
$$
$$
\sum_{i=0}^{n-1} b_{i}B^{i}= 
b_{0}+2\sum_{i=0}^{n-2} b_{i}B^i=
\dots
$$
#### $base 16 \to base 2$

$ABC_{16}\to base_{2}$

Ogni cifra esadecimale può essere rappresentata da 4 bit
quindi $(ABC)_{16}=(1010|1011|1100)_{2}$

### Rappresentazioni 
#### Rappresentazione senza segno
Con 16 bit possiamo rappresentare $[0 , 2^{16}-1]$  -- $(2^{16}-1=65535)$

#### Rappresentazione modulo segno
- 1 bit per il segno
- 15 per il valore
Svantaggi:
- Ha il problema di rappresentare sia $-0$ che $+0$
- Il calcolatore deve sempre controllare il segno dei operandi

Con 16 bit possiamo rappresentare $[-32767,+32767]$

#### Rappresentazione complemento a 1
I numeri negativi vengono rappresentati dalla negazione di tutti i bit del negativo

Questa rappresentazione permette di fare una sottrazione facendo l'operazione di somma

#### Rappresentazione complemento a 2
Il negativo di un numero si ottiene:
- Negando i bit e sommando 1 al numero 
- Oppure sottraendo 1 al numero e poi negandolo bit a bit

Vale l'equazione $X+\bar{X}=-1$

Questa rappresentazione risolve sia il problema del segno che quello della doppia rappresentazione dello zero

Con 16 bit possiamo rappresentare $[-32768,+32767]=[-2^{16},+2^{16}-1]$