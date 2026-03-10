Argomenti: 
- Spazi vettoriali
- Matrici (Denominanti)
- Applicazioni Lineari
- Endomorfismo e Diagonalizzazione

## Esempio
### Mattone
un mattone pesa $1 Kg +\frac{1}{2}mattone$ quanto pesa un mattone? 

$x=1+\frac{1}{2}x\iff\frac{1}{2}x=1\iff x=2$

### Fattoria Galline Capre
- 44 zampe
- 15 teste
Quante capre?, Quante Galline?

$$
x= n-galline
y= n-capre
$$
$$
\begin{aligned}
&\begin{cases}
x=x+y \\
44=2x+4y
\end{cases} \\
\iff 
&\begin{cases}
x=15-y  \\
44 = 30 - 2y +4y \iff 14 = 2y \iff y=7
\end{cases} \\
\iff 
&\begin{cases}
x=15 - y \iff x=15-7 \iff x=8 \\
y=7
\end{cases}
\end{aligned}
$$

## Vettori e n-uple
$\mathbb{R}^n = \mathbb{R}* \mathbb{R}* \dots * \mathbb{R} \implies \text{Insieme delle n-uple o vettori a coefficienti reali}$ 
$\underline{v} \in \mathbb{R}^n$
$\underline{v}=(x_{1},x_{2},\dots,x_{n})$
$x_{1},x_{2},\dots,x_{n}\in\mathbb{r}$

$\underline{v}=\begin{pmatrix}x_{1}\\ \vdots \\x_{n}\end{pmatrix}$

$\mathbb{K}$ Campo --- $(\mathbb{K} =\mathbb{Q,R,C},\mathbb{Z}_{p})$
$\mathbb{k}^n=\text{n-uple di vettori a coefficienti in }\mathbb{k}$

$\underline{v}=\begin{pmatrix}x_{1} \\ \vdots \\ x_{n}\end{pmatrix}\text{ con }x_{1}, x_{2},\dots,x_{n} \in \mathbb{k}$

$\mathbb{k}^n=\left\{\begin{pmatrix}x_{1} \\ \vdots \\ x_{3}\end{pmatrix} : x_{1},\dots,x_{3} \in \mathbb{k}\right\}$

### Osservazione

#### 1

$\underline{x}=\begin{pmatrix}x_{1}\\ \vdots \\ x_{n} \end{pmatrix} \text{, } \underline{y}=\begin{pmatrix}y_{1}\\ \vdots \\ y_{n} \end{pmatrix} \in \mathbb{k}^n x_i,y_i \text{prendono il nome di coordinate o n-uple}$

$\underline{x}=\underline{y}\iff \begin{cases}x_1=y_1\\\vdots\\x_n=y_n\end{cases}$
## Valori scalari e Vettori

$x \in \mathbb{k} \text{scalare}$
$\underline{x} \in \mathbb{k}^n \text{vettori o n-uple}$

### Osservazione

- $\mathbb{k}=\mathbb{k}^1 , x=(x) \text{ sono detti scalari}$ 
## Operazione + 


$$\underline{x}= \begin{pmatrix}x_1\\\vdots\\x_n\end{pmatrix},\underline{y}=\begin{pmatrix}y+1\\\vdots\\y_n\end{pmatrix}$$

$$\begin{aligned}
+: \mathbb{k}^n \times \mathbb{k}^n &\to \mathbb{k} \\
(\underline{x},\underline{y}) &\mapsto \underline{x}+\underline{y}
\end{aligned}
$$
$$
\underline{x}+\underline{y}=
\begin{pmatrix}
x_1 \\
\vdots \\
x_n
\end{pmatrix}
+
\begin{pmatrix}
y_1 \\
\vdots \\
y_n
\end{pmatrix}=
\begin{pmatrix}
x_1+y_1 \\
\vdots \\
x_n+y_n
\end{pmatrix}
$$
### Proprietà +

$$
\left.\begin{align}

\text{associativa: }& \underline{x}+(\underline{y}+\underline{z}= (\underline{x}+\underline{y})+\underline{z}) \\
 \\
\exists \text{ elemento neutro: }& \underline{0}=\begin{pmatrix}
0 \\
\vdots \\
0
\end{pmatrix} : \underline{x}+ \underline{0}=\underline{0}+\underline{x}=\underline{x} \\
 \\
\exists \text{ elemento opposto: }& -\underline{x}=\begin{pmatrix}
-x_1 \\
\vdots \\
-x_n
\end{pmatrix}:\underline{x}+(-\underline{x})= -\underline{x}+\underline{x}= \underline{0} \\
 \\
\text{commutativa: }& \underline{x}+\underline{y}=\underline{y}+\underline{x} \\
\end{align} \right\}
\implies (\mathbb{k},+) \text{gruppo abeliano}
$$

