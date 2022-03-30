# Cadenas de Markov de Tiempo Contínuo {#CMTC}

En esta unidad, consideramos un sistema estocástico que se observa continuamente a lo largo del tiempo, siendo $X_t$ el estado en el momento $t$, con $t \geq 0$. Siguiendo la definición de las CMTD, a continuación definimos las cadenas de Markov de tiempo continuo (CMTC).

::: {.yellowbox data-latex=""}
::: {#cmtc001 .definition}
Un proceso estocástico $\{X_t; t \geq 0\}$ con espacio de estados $S$ es una cadena de Markov de tiempo continuo, CMTC, si para todo $i$ y $j$ en $S$, y $t, s \geq 0$, cumple la propiedad de Markov:

$$P(X_{s+t} = j \mid X_s = i, X_u, 0 \leq u \leq s) = P(X_{s+t} = j \mid X_s = i).$$
:::
:::

La CMTC $\{X_t; t \geq 0\}$ se denomina homogénea si el cambio entre dos instantes cualesquiera depende exclusivamente del tiempo transcurrido, esto es para cualquier $t, s \geq 0$,

$$P(X_{s+t} = j \mid X_s = i) = P(X_t = j \mid X_0 = i).$$

En toda esta unidad asumimos que las CMTC con las que trabajamos son homogéneas y tienen espacio de estados finito $S=\{1, 2,,...,N\}$ de forma que podemos definir la probabilidad de pasar del estado $i$ al estado $j$ en un periodo de amplitud $t, (0,t],$ como:

$$p_{ij}(t) = P(X_t = j \mid X_0 = i), \quad 1 \leq i, j \leq N.$$

La matriz de probabilidad de transición $P(t)$ de una CMTC $\{X_t; t \geq 0\}$ viene dada por las distribuciones de probabilidad condicionadas (por filas) de pasar de un estado $i$ a un estado $j$ en un periodo de tiempo de amplitud $t$:

$$P(t) = 
\begin{pmatrix}
p_{11}(t) & p_{12}(t) & ... & p_{1N}(t)\\
p_{21}(t) & p_{22}(t) & ... & p_{2N}(t)\\
... & ... & ... & ...\\
p_{N1}(t) & p_{N2}(t) & ... & p_{NN}(t)
\end{pmatrix}$$

Dicha matriz de transición verifica que:

-   Todos sus elementos son probabilidades condicionadas $\{p_{ij}(t); j \in S\}$

$$1 \geq p_{ij}(t) \geq 0, \quad  1 \leq i, j \leq N; t \geq 0$$

-   La suma de las probabilidades en cada fila, esto es, de acceder a cualquiera de los estados a partir de un estado $i$ es igual a 1.

$$\sum_{j=1}^N p_{ij}(t) = 1, \quad  1 \leq i, j \leq N; t \geq 0$$

-   Ecuaciones de Chapman-Kolmogorov

$$p_{ij}(s+t) =  \sum_{k=1}^N p_{ik}(s)p_{kj}(t) =  \sum_{k=1}^N p_{ik}(t)p_{kj}(s)   , \quad  1 \leq i, j \leq N; t \geq 0$$


La dificultad principal con $P(t)$ es que resulta díficil de obtener de forma inmediata para la mayoría de las CMTC, al contrario de lo que ocurría con las CMTD. Necesitamos un método simple que nos permita describir de forma rápida el comportamiento del proceso. A continuación, sentamos las bases para el estudio de las CMTC a partir de los tiempos de permanencia en cada uno de los estados del proceso. Dado que la única distribución que verifica la propiedad de pérdida de memoria es la exponencial, la utilizaremos como base para la construcción de una CMTC.

## Evolución del proceso {#CMTCA}

Sea $X_t$ el estado de un sistema en el instante temporal $t$. Supongamos que el espacio de estados del proceso estocástico $\{X_t; t \geq 0\}$ es $S=\{1, 2,...,N\}$. La evolución aleatoria del sistema se produce de la siguiente manera:

-   Supongamos que el sistema comienza en el estado $i$ y permanece allí durante un tiempo $Exp(r_i)$ que denominamos **tiempo de permanencia** en el estado $i$, con $r_i$ la **tasa media de permanencia**; recordemos que en la distribución exponencial las tasas medias son el recíproco de los tiempos medios.

-   Al final del tiempo de permanencia en el estado $i$, el sistema realiza una transición repentina al estado $j$ con probabilidad $p_{ij}$, independientemente del tiempo que el sistema haya permanecido en el estado $i$. Una vez en el estado $j$, permanece allí durante un tiempo $Exp(r_j)$.

-   A continuación pasa a un nuevo estado $k$ con una probabilidad $p_{jk}$, independientemente de la historia del sistema hasta el momento, y repite este comportamiento hasta que finaliza el tiempo de observación del proceso.

Conviene hacer tres observaciones con respecto al funcionamiento del sistema:

- En primer lugar, las probabilidades de salto $p_{ij}$ no deben deben confundirse con las probabilidades de transición $p_{ij}(t)$. En este caso, $p_{ij}$ actúa como la probabilidad de que el sistema pase al estado $j$ cuando sale del estado $i$. 
- En segundo lugar, $p_{ii} = 0$, dado que, por definición, el tiempo de permanencia en el estado $i$ es el tiempo que el sistema pasa en el estado $i$ hasta que sale de él; por lo tanto, no es posible una transición de $i$ a $i$. 
- En tercer lugar, en caso de que el estado $i$ sea absorbente, es decir que el sistema permanezca en ese estado para siempre una vez que llegue a él, fijamos $r_i = 0$.

Con estas premisas podremos obtener la **matriz de probabilidades de salto**, $P$, que denotaremos como:

$$P = 
\begin{pmatrix}
p_{11} & p_{12} & ... & p_{1N}\\
p_{21} & p_{22} & ... & p_{2N}\\
... & ... & ... & ...\\
p_{N1} & p_{N2} & ... & p_{NN}
\end{pmatrix}$$


::: {#thecmtc001 .theorem}
El proceso estocástico $\{X_t; t \geq 0\}$ con parámetros $r_i$, $1 \leq i \leq N$, y probabilidades $p_{ij}$, $1 \leq i,j \leq N$ descrito anteriormente es una CMTC.
:::

A continuación presentamos un par de ejemplos.

::: {.example #excmtc001}
**Sistema de vida útil de un satélite**. Supongamos que la vida útil $T$ de un satélite de gran altitud es una variable aleatoria exponencial de tasa $\mu$ en meses, $Exp(\mu)$, de forma que una vez que falla sigue fallando para siempre, ya que no es posible repararlo. Consideramos el proceso $X_t = 1$ si el satélite está operativo en el momento $t$, y 0 en caso contrario. En esta situación $r_0 = 0$ (porque si se estropea, se queda estropeado) y $r_1 = \mu$ (que es el tiempo esperado de vida), pero desconocemos los valores de $P$, aunque podremos obtener la matriz de transición calculando las probabilidades $p_{00}(t)$ y $p_{11}(t)$ que vienen dadas por:

$$p_{00}(t) = P(\text{satélite no está operativo en t} \mid \text{satélite no está operativo en 0}) = 1$$

\begin{eqnarray*}
p_{01}(t) &=& P(X_t=1|X_0=0) = 0 \\
p_{10}(t) &=& P(X_t=0|X_0=1) = Pr(T \leq t) = 1- e^{-\mu t}\\
p_{11}(t) &=& P(X_t=1|X_0=1) = Pr(T >t) = e^{-\mu t}
\end{eqnarray*}

La matriz de transición viene dada pues por:

$$P(t) = 
\begin{pmatrix}
1 & 0\\
1- e^{-\mu t} & e^{-\mu t} 
\end{pmatrix}$$
:::


::: {.example #excmtc002}
**Sistema del viajante**. Un vendedor vive en la ciudad A y es responsable de las ciudades A, B y C. El tiempo que pasa en cada ciudad es aleatorio. Tras un estudio, se ha determinado que la cantidad de tiempo consecutivo que pasa en una ciudad cualquiera sigue una variable aleatoria con distribución exponencial, cuya media de tiempo de estancia depende de la ciudad. En su ciudad natal pasa un tiempo medio de dos semanas, en la ciudad B pasa un tiempo medio de una semana, y en la ciudad C pasa un tiempo medio de una y media. Cuando sale de la ciudad A, lanza una moneda para determinar a qué ciudad va a continuación; cuando sale de la ciudad B o C, lanza dos monedas de manera que hay un 75% de posibilidades de volver a A y un 25% de posibilidades de ir a la otra ciudad. Sea $X_t$ una variable aleatoria que denota la ciudad en la que se encuentra el vendedor en el momento $t$, de forma que toma el valor 0 si está en A, el valor 1 si está en B, y 2 si está en C.
:::

El proceso $\{X_t; t \geq 0\}$ con espacio de estados $\{0, 1, 2\}$ es una CMTC con:

-   tasas de permanecia (en semanas): $r_0 = 1/2=0.5, r_1 = 1, r_2 = 1/1.5 =0.67$ , y
-   matriz de saltos

$$P = 
\begin{pmatrix}
0 & 0.5 & 0.5\\
0.75 & 0 & 0.25\\
0.75 & 0.25 & 0
\end{pmatrix}$$



## Descripción del proceso {#CMTCB}

En virtud del teorema \@ref(thm:thecmtc001) todas las CMTC con espacios de estado finitos que tienen tiempos de permanencia no nulos en cada estado pueden ser descritos a través de las tasas de permanencia y la matriz de saltos. En este punto detallamos este análisis e introducimos todos los conceptos necesarios para el análisis del proceso.

La **tasa de transición** de $i$ a $j$ se define como el producto de la tasa de permanencia en el estado $i$, $r_i$, por la probabilidad de salto al estado $j$, $p_{ij}$ 

\begin{equation}
r_{ij} = r_i p_{ij}
\end{equation}

De forma análoga a las CMTD, una CMTC también puede representarse gráficamente mediante un grafo dirigido cuyos nodos (o vértices) indican cada uno de los estados del proceso, y surge un arco dirigido del nodo $i$ al nodo $j$ si $p_{ij} > 0$; además junto a cada arco se escribe la tasa de transición $r_{ij} = r_i p_{ij}$. Nunca habrá arcos que empiecen y acaben en el mismo nodo porque $p_{ii}=0$. Esta representación gráfica se denomina **diagrama de tasas de la CMTC**.

Podemos entender la dinámica de una CMTC visualizando una partícula que se mueve de nodo en nodo en el diagrama de tasas de la siguiente manera: permanece en el nodo $i$ durante cierto periodo de tiempo de duración variable $Exp(r_i)$ y luego elige uno de los arcos de salida del nodo $i$ con probabilidades proporcionales a las tasas de los arcos, trasladándose al nodo que conecta dicho arco con el nodo origen $i$. Este movimiento continúa para siempre. El nodo ocupado por la partícula en el instante $t$ es el estado de la CMTC en el instante $t$.

En esta situación resulta posible obtener los valores de $r_i$ y $p_{ij}$ a partir de las tasas $r_{ij}$ dado que:

\begin{eqnarray*}
\sum_{j=1}^{N} r_{ij} &=& \sum_{j=1}^{N} r_i p_{ij} = r_i \sum_{j=1}^{N} p_{ij} = r_i\\
p_{ij} &=& \frac{r_{ij}}{r_i} \quad \text{ si } r_i \neq 0.
\end{eqnarray*}

Para un mejor manejo de la información, resulta conveniente construir la **matriz de tasas de permanencia** (o tasas de ocupación) teniendo en cuenta que $r_{ii} = 0$ para cualquier valor de $i$, y por tanto la diagonal de la matriz $R$ es siempre cero. Así tenemos que:

$$R = 
\begin{pmatrix}
0 & r_{12} & ... & r_{1N}\\
r_{21} & 0 & ... & r_{2N}\\
... & ... & ... & ...\\
r_{N1} & r_{N2} & ... & 0
\end{pmatrix}$$


A partir de la matriz $R$ se puede obtener la denominada **matriz generadora** $Q$ de la CMTC, que se define como aquella que tiene por elementos $q_{ij}$, con:

$$q_{ij} = \begin{cases}
-r_i, \quad \text{ si } i = j, \\
r_{ij}, \quad \text{ si } i \neq j.
\end{cases}$$ 


de forma que:

$$Q = 
\begin{pmatrix}
-r_1 & r_{12} & ... & r_{1N}\\
r_{21} & -r_2 & ... & r_{2N}\\
... & ... & ... & ...\\
r_{N1} & r_{N2} & ... & -r_N
\end{pmatrix}$$

::: {#excmtc003 .example}
Continuando con el sistema de vida útil del satélite del ejemplo \@ref(exm:excmtc001), podemos establecer que $r_0 = 0$ y $r_1 = \mu$, con $p_{10} = 1$ y $p_{01}$ no definida, de forma que:
:::

$$R = 
\begin{pmatrix}
0 & 0 \\
\mu & 0
\end{pmatrix} \quad \text{ y } \quad Q = 
\begin{pmatrix}
0 & 0 \\
\mu & -\mu
\end{pmatrix}$$

En esta situación el diagrama de tasas se construye con la librería `diagram`:

\begin{figure}

{\centering \includegraphics[width=0.95\linewidth]{04-CMTC_files/figure-latex/05-002-1} 

}

\caption{Diagrama de tasas para el tiempo de vida del Satélite}(\#fig:05-002)
\end{figure}



::: {#excmtc004 .example}
Continuando con el sistema del viajante descrito en el ejemplo \@ref(exm:excmtc002) ya que conocemos las tasas medias y las probabilidades de salto podemos obtener la matriz $R$ de forma inmediata:
:::


```r
estados <- c("0", "1", "2")
nestados <- length(estados)

P <- matrix(nrow = nestados, ncol = nestados, 
            data = c(0, 0.5, 0.5, 0.75, 0, 0.25, 0.75, 0.25, 0), 
            byrow = 3)
r <- matrix(nrow = nestados, ncol = nestados, 
            data = rep(c(0.5, 1, 1.5),3))

R <- P*r
R
```

```
##       [,1]  [,2] [,3]
## [1,] 0.000 0.250 0.25
## [2,] 0.750 0.000 0.25
## [3,] 1.125 0.375 0.00
```

de forma que el diagrama de tasas viene dado por (asignamos el valor de la ciudad a cada uno de los posibles estados del sistema)

\begin{figure}

{\centering \includegraphics[width=0.95\linewidth]{04-CMTC_files/figure-latex/05-004-1} 

}

\caption{Diagrama de tasas para el proceso del vendedor}(\#fig:05-004)
\end{figure}

El diagrama representa el comportamiento de todo el proceso de viajes y estancias del vendedor.



::: {.example #excmtc005}
**Sistema vida útil de una máquina**. Consideramos un sistema compuesto por una máquina que funciona durante un cantidad de tiempo que viene determianda por una variable aleatoria $Exp(\mu)$ hasta que falla. Una vez se detecta la avería, la máquina se repara. El tiempo de reparación es una variable aleatoria $Exp(\lambda)$ y es independiente del pasado. La máquina está como nueva después de la reparación. Sea $X(t)$ el estado de la máquina en tiempo $t$, de forma que toma el valor 1 si está en marcha y 0 si está parada (porque está siendo reparada).
:::

En esta situación el tiempo de estancia en el estado 0 es el tiempo de reparación, de forma que $r_0 = \lambda$, mientras que el tiempo en el estado 1 es el tiempo de funcionamiento con $r_1 = \mu$. Además las probabiliddes de salto de interés son $p_{01} = 1$ y $p_{10} = 1$, dado que la máquina siempre es reparada y vuelve a funcionar, y porque sabemos que la máquina debe estropearse en algún momento.

El proceso definido de esta forma $\{X_t; t \geq 0\}$ es una CMTC cuya matriz de tasas y matriz generadora del sistema vienen dadas por:

$$R = 
\begin{pmatrix}
0 & \lambda \\
\mu & 0
\end{pmatrix} \quad \text{ y } \quad Q = 
\begin{pmatrix}
-\lambda & \lambda \\
\mu & -\mu
\end{pmatrix}$$

El diagrama del sistema viene dado por:

\begin{figure}

{\centering \includegraphics[width=0.95\linewidth]{04-CMTC_files/figure-latex/05-005-1} 

}

\caption{Diagrama de tasas para el sistema de una máquina}(\#fig:05-005)
\end{figure}



## Análisis preliminar del proceso {#CMTCC}

Aunque más adelante estudiaremos los aspectos teóricos para el análisis completo de una CMTC, en este punto utilizamos la simulación del sistema para analizar su comportamiento. Nos centramos en las herramientas de simulación estudiadas hasta este punto para más adelante presentar con detalle la librería `simmer` que nos permite simular procesos y sistemas complejos.

Utilizamos los ejemplos \@ref(exm:excmtc002) y \@ref(exm:excmtc005) para mostrar cómo analizar un sistema, dado que el descrito en el ejemplo \@ref(exm:excmtc001) se puede analizar sin más que describir la tasa del tiempo de vida del satélite. 

### Sistema de vida útil de una máquina

Comenzamos con el ejemplo \@ref(exm:excmtc005), para el que vamos a construir un algoritmo con el que simular el sistema hasta cierto instante de tiempo.


::: {.silverbox data-latex=""}
Algoritmo para el sistema de vida útil de una máquina:

1.  Fijar tasas de funcionamiento $\mu$ y reparación $\lambda$, así como el tiempo en que el sistema estará funcionando ($tfin$).
2.  Fijar el tiempo de funcionamiento $tfun = 0$, tiempo de reparación $trep = 0$, y tiempo de funcionamiento del sistema $tsis = tfun + trep$.
3.  Fijar el número de vistas al estado de funcionamiento $nfun = 0$ y al estado de reparación $nrep =0$.

Repetir los pasos siguientes hasta abandonar el sistema:

3.  Generar $tfun \sim Exp(\mu)$ actualizando $tsis$ y $nfun$.
4.  Si $tsis > tfin$, abandonar el sistema.
5.  Generar $trep \sim Exp(\lambda)$ actualizando $tsis$ y $nrep$.
6.  Si $tsis > tfin$, abandonar el sistema.

Los valores $tfun$, $trep$, así como las veces que se visitan los estados de funcionamiento y reparación nos permiten describir el funcionamiento del sistema para un tiempo prefijado.
:::

Para facilitar el análisis establecemos que todos los tiempos del sistema están en días y que deseamos estudiar el sistema durante un año. Creamos una función que nos permite modificar los valores de la tasa del tiempo de funcionamiento (recíproco de la media de tiempo en funcionamiento ), la tasa de reparación (recíproco de la media del tiempo de reparación) y el tiempo total de funcionamiento del sistema. Almacenamos los resultados de cada paso por el sistema para poder realizar los análisis correspondientes.


```r
TSIM_one_machine <- function(tasafun, tasarep, tfin)
{
  # Parámetros de la función
  # =========================
  # tasafun: tasa de funcionamiento
  # tasarep: tasa de reparación
  # tfin: tiempo de funcionamiento del sistema
  
  # inicialización de parámetros del sistema 
  tfun = trep = nfun = nrep = tsis = vector()
  # estado inicial del sistema
  i <- 1
  tfun[i] = trep [i] = nfun[i] = nrep[i] = 0
  tsis[i] <- tfun[i] + trep[i]
  
  # Fijamos semilla
  set.seed(123)
  # Bucle de simulación
  while(tsis[i] <= tfin)
  {
    i<- i + 1
    # Máquina en funcionamiento
    nfun[i] = nfun[i-1] + 1
    tfun[i] = rexp(1, tasafun) 
    tsis[i] = tsis[i-1] + tfun[i]
    trep[i] = 0 #Actualizamos estos dos parámetros ya que no hemos entrado en reparación
    nrep[i] = nrep[i-1]
    if(tsis[i] > tfin) {
      # adaptamos valores para quedarnos en los 365 días
      tfun[i] <- tfin - tsis[i-1]
      tsis[i] <- tsis[i-1] + tfun[i]
      break
      }
    # Máquina en reparación
    nrep[i] = nrep[i-1] + 1
    trep[i] = rexp(1, tasarep)
    tsis[i] = tsis[i] + trep[i]
    if(tsis[i] > tfin) {
    # adaptamos valores para quedarnos en los 365 días
      trep[i] <- tfin - tsis[i-1] 
      tsis[i] = tsis[i] + trep[i]
      break
      }
  }
  res <- tibble(tfun, nfun, trep, nrep, tsis)
  # Devolvemos resultados del sistema quitando la fila de inicialización
  return(res[-1,])
}
```

Supongamos que a través de los registros históricos de funcionamiento y reparación de la máquina se sabe que el tiempo medio de funcionamiento es de 60 días ($\mu = 1/60$) y el tiempo medio de reparación es de cuatro días ($\lambda = 1/4$). Además se está interesado en estudiar el funcionamiento del sistema para el próximo año (365 días). Queremos pues, estimar:

-   Proporción del tiempo que la máquina está funcionando y en reparación.
-   Número de ocasiones en que la máquina debe ser reparada.
-   Si el beneficio neto es de 100 euros por cada día que la máquina está funcionando y una pérdida de 1500 euros por cada día que está en reparación ¿cuál es el beneficio esperado para el próximo año?

Obtenemos la simulación del sistema:


```r
mu <- 1/60
lambda <- 1/4
simulacion <- TSIM_one_machine(mu, lambda, 365)
simulacion
```

```
## # A tibble: 6 x 5
##     tfun  nfun  trep  nrep  tsis
##    <dbl> <dbl> <dbl> <dbl> <dbl>
## 1  50.6      1 2.31      1  52.9
## 2  79.7      2 0.126     2 133. 
## 3   3.37     3 1.27      3 137. 
## 4  18.9      4 0.581     4 157. 
## 5 164.       5 0.117     5 321. 
## 6  44.5      6 0         5 365
```

Podemos ver que el número de ciclos en que la máquina esta en fucionamiento es 6 mientras que el número de veces que ha necesitado reparación son 5.

Calculamos ahora los tiempos totales de funcionamiento y reparación:


```r
tiempos <- apply(simulacion[c(1,3)], 2, sum)
tiempos
```

```
##       tfun       trep 
## 360.603564   4.396436
```

Por tanto, la proporción de tiempo que la máquina está en funcionamiento es 0.99, y el beneficio estimado para el próximo año viene dado por:


```r
beneficio <- 100 * tiempos["tfun"] - 1500 * tiempos["trep"]
beneficio
```

```
##    tfun 
## 29465.7
```

### Sistema del viajante 

A continuación analizamos el sistema del viajante correspondiente al ejemplo \@ref(exm:excmtc002). En primer lugar establecemos el algoritmo de simulación del sistema. Para facilitar todas las posibilidades del algoritmo asumimos que el vendedor comienza el recorrido en la ciudad en la que reside.


::: {.silverbox data-latex=""}
Algoritmo para el análisis del sistema del viajante:

1.  Fijar tasas de permanencia en cada ciudad $\mu_A$, $\mu_B$, y $\mu_C$, así como las probabilidades de salto dadas en la matriz $P$, y una varibale que indica la ciudad en la que nos encontramos ($ciudad$).
2.  Fijar el tiempo de funcionamiento del sistema $tsis = 0$, tiempo de permanencia en cada ciudad $tiempo = 0$, y el tiempo de estudio $tfin$.
3.  Generar $tiempo \sim Exp(\mu_a)$ y actualizar $tsis$, de forma que si $tsis > tfin$ abandonamos el sistema.
4.  Generamos un salto de la ciudad $A$ de acuerdo a las probabilidades de $P$ correspondientes a la ciudad $A$.

Repetir los pasos siguientes hasta que el tiempo en el sistema supere el tiempo fijado:

5.  Actualizamos $ciudad$, generamos $tiempo \sim Exp(\mu_{ciudad})$ y actualizamos $tsis$, de forma que si $tsis > tfin$ abandonamos el sistema.
6.  Generamos un salto de la ciudad del paso anterior de acuerdo a las probabilidades de $P$ correspondientes a dicha ciudad.

Los valores $tiempo$, y $ciudad$ nos permiten describir el funcionamiento del sistema para un tiempo prefijado.
:::

A continuación construimos la función para simular el sistema hasta cierto instante de tiempo.


```r
TSIM_viajante <- function(tasaA, tasaB, tasaC, tfin)
{
  # Parámetros de la función
  # =========================
  # tasaA: tasa de permanencia en A
  # tasaB: tasa de permanencia en B
  # tasaC: tasa de permanencia en C
  # tfin: tiempo de funcionamiento del sistema
  
  # inicialización de parámetros del sistema 
  tiempo = tsis = ciudad = vector()
  # Probabilidades de salto. Fijamos la primera ya que la otra es complementaria
  pA <- 0.5 # de A a B. De A a C es  1-pA
  pB <- 0.75 # de B a A. De B a C es  1-pB
  pC <- 0.75 # de C a A. De C a B es  1-pC
  
  # estado inicial del sistema
  i <- 1
  ciudad[i] <- "A"
  # Fijamos semilla
  set.seed(123)
  # Primer tiempo de estancia
  tiempo[i] = rexp(1, tasaA)
  tsis[i] <- tiempo[i]
  # Saltamos de la ciudad A
  uniforme <- runif(1)
  ifelse(uniforme <= pA, ciudad[i+1] <- "B", ciudad[i+1] <- "C")
      
  # Bucle de simulación
  while(tsis[i] <= tfin)
  {
    i<- i + 1
    uniforme <- runif(1)
    if(ciudad[i] == "A")
      {
        # Calculamos tiempo de permanencia
        tiempo[i] <- rexp(1, tasaA)
        # Actualizamos y valoramos si hemos alcanzado el tiempo límite
        tsis[i] = tsis[i-1] + tiempo[i]
        if(tsis[i] > tfin){
            tiempo[i] <- tfin - tsis[i-1] 
            tsis[i] = tsis[i-1] + tiempo[i]
            break
          }
        # Si no hemos alcanzado el límite realizamos un nuevo salto
        ifelse(uniforme <= pA, ciudad[i+1] <- "B", ciudad[i+1] <- "C")
      }
    if(ciudad[i] == "B")
      {
        # Calculamos tiempo de permanencia
        tiempo[i] <- rexp(1, tasaB)
        # Actualizamos y valoramos si hemos alcanzado el tiempo límite
        tsis[i] = tsis[i-1] + tiempo[i]
        if(tsis[i] > tfin){
            tiempo[i] <- tfin - tsis[i-1] 
            tsis[i] = tsis[i-1] + tiempo[i]
            break
          }
        # Si no hemos alcanzado el límite realizamos un nuevo salto
        ifelse(uniforme <= pB, ciudad[i+1] <- "A", ciudad[i+1] <- "C")
      }  
    if(ciudad[i] == "C")
      {
        # Calculamos tiempo de permanencia
        tiempo[i] <- rexp(1, tasaC)
        # Actualizamos y valoramos si hemos alcanzado el tiempo límite
        tsis[i] = tsis[i-1] + tiempo[i]
        if(tsis[i] > tfin){
            tiempo[i] <- tfin - tsis[i-1] 
            tsis[i] = tsis[i-1] + tiempo[i]
            break
          }
        # Si no hemos alcanzado el límite realizamos un nuevo salto
        ifelse(uniforme <= pC, ciudad[i+1] <- "A", ciudad[i+1] <- "C")
      }
  }
  # Devolvemos resultados del sistema 
  ciudad <- factor(ciudad)
  res <- tibble(tiempo, ciudad, tsis)
  return(res)
}
```

Supongamos que estamos interesados en aproximar el comportamiento del vendedor durante el próximo año (52 semanas) para poder contestar a las preguntas siguientes:

-   Proporción de tiempo que el vendedor pasa en cada ciudad.
-   Número de ocasiones en que visita cada ciudad.

Simulamos pues el sistema y contestamos a las preguntas planteadas:


```r
tasaA <- 1/2
tasaB <- 1/1
tasaC <- 2/3
tfin <- 52
simulacion <- TSIM_viajante(tasaA, tasaB, tasaC, tfin)  
# Análsis del sistema
simulacion %>% 
  group_by(ciudad) %>% 
  summarise(visita = n(), estancia = sum(tiempo)) %>%
  mutate(proporcion = estancia/52)
```

```
## # A tibble: 3 x 4
##   ciudad visita estancia proporcion
##   <fct>   <int>    <dbl>      <dbl>
## 1 A          16     30.3      0.583
## 2 B           8     10.7      0.206
## 3 C          11     11.0      0.211
```
### Análisis con simmer

Si bien dedicamos en este curso un capítulo entero para contar el funcionamiento de la librería `simmer`, puesto que estos ejemplos son relativamente sencillos, podemos introducir sin demasiada complicación el algoritmo de simulación con `simmer`. 

La libreria `simmer` permite simular de sistemas tanto continuos como discretos bajos dos premisas fundamentales:

-   Proceso de llegadas ("arrivals") al sistema.
-   Proceso de servicio ("server"): en el que a cada una de las llegadas se les asigna una serie de actividades a realizar (integradas en 'trayectorias') y unos recursos o servidores que resuelven con ellas las actividades.

Para el sistema descrito en ejemplo \@ref(exm:excmtc005) el proceso de llegadas viene identificado por las averías que se producen en determinados instantes de tiempo. El proceso de servicio se corresponde con las reparaciones de las máquinas, a las que se dedica cierto tiempo. En este caso el recurso es el reparador encargado de resolver la avería y poner en marcha la máquina de nuevo.

El primer paso del algoritmo de simulación es cargar las librerías y definir la semilla de simulación:


```r
library(simmer)
library(simmer.plot)
library(simmer.bricks)
set.seed(1234)
```

Definimos ahora el entorno de simulación del sistema:


```r
# Tasas de permanencia del sistema
##################################
lambda <- 1/4  # tasa reparación
mu <- 1/60     # tasa funcionamiento

# Sistema
#################################################
sistema.1m <- function(t, lambda, mu)
{
  # tarea dentro de sistema: reparación de las averías
  reparar <- trajectory() %>%
    # la máquina estropeada se asigna a un reparador
    seize("reparador", amount = 1) %>%              
    # el tiempo de reparación es aleatorio
    timeout(function() rexp(1, lambda)) %>%   
    # la máquina ya ha sido reparada
    release("reparador", amount = 1)               

  # Configuración del sistema 
  #################################################
  simmer() %>%
    # Se definen los recursos: un único reparador y cola infinita
    add_resource("reparador", capacity = 1) %>%           
    # Simulador de los tiempos entre averías, dirigidas a la trayectoria "reparar"
    add_generator("averia", reparar, function() rexp(1, mu)) %>% 
    # Tiempo funcionamiento del sistema
    run(until = t)     
}

### Simulación del sistema durante 365 días
operar <- sistema.1m(365, 1/4, 1/60)
```

Analizamos ahora los resultados que proporciona el sistema simulado. Podemos acceder a las información de dos formas diferentes mediante las funciones `get_mon_arrivals()` para describir las llegadas (averías) y `get_mon_resources()` para describir la ocupación de los servidores recursos (técnicos reparadores).


```r
reparacion = get_mon_arrivals(operar)
reparacion
```

```
##      name start_time end_time activity_time finished replication
## 1 averia0   150.1055 150.1318    0.02632783     TRUE           1
## 2 averia1   164.9110 166.4598    1.54873033     TRUE           1
## 3 averia2   269.4758 272.7721    3.29632606     TRUE           1
## 4 averia3   274.8728 278.2250    3.35216128     TRUE           1
## 5 averia4   287.0299 294.5502    7.52030671     TRUE           1
## 6 averia5   332.6557 339.2903    6.63464954     TRUE           1
```

El objeto resultante tiene las columnas siguientes:

-   `name`: nombre de las llegadas (averías), numeradas correlativamente
-   `star_time`: instante en el que llega al sistema (se produce la avería)
-   `end_time`: instante en el que sale del sistema (se ha reparado)
-   `activity_time`: tiempo dedicado a la tarea (tiempo de reparación)
-   `finished`: si la tarea ha finalizado dentro del periodo de tiempo de simulación establecido.
-   `replication`: número de replicas del sistema (1 porque sólo hemos lanzado una cadena).


```r
recursos = get_mon_resources(operar)
recursos
```

```
##     resource     time server queue capacity queue_size system limit replication
## 1  reparador 150.1055      1     0        1        Inf      1   Inf           1
## 2  reparador 150.1318      0     0        1        Inf      0   Inf           1
## 3  reparador 164.9110      1     0        1        Inf      1   Inf           1
## 4  reparador 166.4598      0     0        1        Inf      0   Inf           1
## 5  reparador 269.4758      1     0        1        Inf      1   Inf           1
## 6  reparador 272.7721      0     0        1        Inf      0   Inf           1
## 7  reparador 274.8728      1     0        1        Inf      1   Inf           1
## 8  reparador 278.2250      0     0        1        Inf      0   Inf           1
## 9  reparador 287.0299      1     0        1        Inf      1   Inf           1
## 10 reparador 294.5502      0     0        1        Inf      0   Inf           1
## 11 reparador 332.6557      1     0        1        Inf      1   Inf           1
## 12 reparador 339.2903      0     0        1        Inf      0   Inf           1
```

Con la función `get_mon_resources()` tenemos una descripción continua de los recursos del proceso con las columnas:

-   `resource`: recurso (numerado correlativamente si hubiera más de uno)
-   `time`: instante de tiempo en que se ha registrado alguna actividad (avería, reparación, puesta en marcha)
-   `server`: número de servidores (recursos) ocupados (como sólo hay un técnico, es igual a 1)
-   `queue`: llegadas en la cola (averías que están esperando ser reparadas porque el técnico está ocupado)
-   `capacity`: capacidad del sistema o número de reparadores en el sistema
-   `queue_size`: tamaño de la cola de espera (averías en espera para ser reparadas)
-   `system`: llegadas en el sistema (número de averías en ese instante, en reparación o en cola)
-   `limit`: capacidad + tamaño de la cola
-   `replication`: número de replica del sistema (1 porque sólo se ha lanzado una cadena).

A partir de los resultados podemos responder a las mismas preguntas que ya planteamos antes:

-   Proporción del tiempo que la máquina está funcionando y en reparación.
-   Número de ocasiones en que la máquina debe ser reparada.
-   Si el beneficio neto es de 100 euros por cada día que la máquina está funcionando y una pérdida de 1500 euros por cada día que está en reparación ¿cuál es el beneficio esperado para el próximo año?

Para responder al primer pregunta basta con sumar los tiempos de actividad y calcualr su proporción sobre los 365 días de simulación:


```r
tiempo_reparacion <- sum(reparacion$activity_time)
propor <- round(100*(1-(tiempo_reparacion/365)), 2)
propor
```

```
## [1] 93.87
```

La proporción de tiempo en que la máquina está funcionando es del 93.87% y el tiempo que está en reparación es del 6.13%.

El número de ocasiones en que la máquina está en reparación corresponde al número de veces que accedemos a la tarea de reparación, que en este caso es de 6.

Para calcular el beneficio obtenido a lo largo del año utilizamos el tiempo de reparación obtenido:


```r
(365-tiempo_reparacion)*100 - tiempo_reparacion*1500
```

```
## [1] 694.3972
```

Como se puede ver, los resultados no son muy similares a los obtenidos con el algoritmo que programamos nosotros. Para conseguir estimaciones estables deberíamos realizar diferentes replicaciones del sistema (lanzar varias cadenas) y promediar los beneficios obtenidos mediante un estimador Monte-Carlo. Podemos replicar fácilmente el sistema añadiendo dos líneas de código. Cada cadena replicada tendrá un distintivo diferente en el argumento 'replicate' cuando monitorizamos los resultados.


```r
# lanzamos 'nreplicas' de la cadena, que se almacenan en una lista
nreplicas <- 500
envs <- lapply(1:nreplicas, function(i){
  operar <- sistema.1m(365, 1/4, 1/60)
})
```

Ahora almacenamos todas las simulaciones a modo de matriz, en concreto 'tibble', para poder realizar los cálculos oportunos agrupando por réplicas y con ellos poder calcular las estimaciones Monte-Carlo del tiempo total de reparación y del número de reparaciones.


```r
# guardamos todas las llegadas en formato 'tibble'
simulaciones <- as_tibble(get_mon_arrivals(envs))

# agrupamos por 'replication' para calcular los descriptivos de interés en cada réplica
salida <- simulaciones %>% 
  group_by(replication) %>% 
  # Calculamos los descriptivos por cada réplica
  summarise(nreparaciones = n(), 
            tiempoparada = sum(activity_time)) %>% 
  # Calculamos las estimaciones Monte-Carlo con los descriptivos de las réplicas
  summarise(mmedia_nrepara = mean(nreparaciones),
            min_nrepara = min(nreparaciones),
            max_nrepara = max(nreparaciones),
            media_taveria = mean(tiempoparada),
            q25_taveria = quantile(tiempoparada, 0.25),
            q50_taveria = quantile(tiempoparada, 0.50),
            q75_taveria = quantile(tiempoparada, 0.75),
            sd_taveria = sd(tiempoparada) 
            )
salida
```

```
## # A tibble: 1 x 8
##   mmedia_nrepara min_nrepara max_nrepara media_taveria q25_taveria q50_taveria q75_taveria
##            <dbl>       <int>       <int>         <dbl>       <dbl>       <dbl>       <dbl>
## 1           6.12           1          15          23.8        13.4        21.3        31.9
## # ... with 1 more variable: sd_taveria <dbl>
```

En 'mmedia_nrepara' tenemos la estimación del número de reparaciones en un año, en 'min_nrepara' y 'max_nrepara' el mínimo y el máximo respectivamente; en 'media_taveria' tenemos una estimación del tiempo medio acumulado en reparaciones en un año, con sus cuartiles (q25, q50 y q75) y desviación típica (sd).

Así podemos calcular los beneficios esperados en función del número de días que la máquina funciona y los que está estropeada:


```r
# número de días en funcionamiento vs número de días en reparación
(365-salida$media_taveria)*100 - salida$media_taveria*1500
```

```
## [1] -1561.395
```

Podemos considerar también otros dos escenarios posibles: uno pesimista, con un mayor tiempo acumulado de reparación (cuantil 75), y uno optimisma, con un menor tiempo (con el cuantil 25), y calcular con ellos los beneficios:


```r
(365-salida$q75_taveria)*100 - salida$q75_taveria*1500
```

```
##       75% 
## -14480.82
```

```r
(365-salida$q25_taveria)*100 - salida$q25_taveria*1500
```

```
##      25% 
## 15047.35
```

¿Qué conclusiones podemos extraer de este análisis?

Para finalizar este apartado presentamos un nuevo ejemplo donde el espacio de estados está compuesto por una pareja de valores y no por un único valor como en los ejemplos que hemos presentado hasta ahora.

### Mantenimiento de aronaves

Una empresa de mantenimiento de aeronaves está interesada en el proceso de avería-reparación de cierto tipo de aviones. El tipo de avión de interés es un avión comercial a reacción con cuatro motores, dos en cada ala. Cuando un motor se enciende, el tiempo que se puede mantener en funcionamiento hasta que falla es una variable aleatoria exponencial con parámetro $\lambda$. Si el fallo se produce en vuelo, no puede haber reparación, pero el avión necesita al menos un motor en cada ala para funcionar correctamente y poder volar con seguridad. En concreto, la empresa está interesada en poder predecir la probabilidad de un vuelo sin problemas.

Si denotamos por $X_L(t)$ y $X_R(t)$ el número de motores funcionando en el instante $t$ en el ala izquierda y el ala derecha respectivamente, podemos considerar el estado del sistema en el instante $t$ como $X(t) = (X_L(t), X_R(t))$. Si asumimos que los fallos en los motores son independientes entre sí, podemos ver que el proceo $\{X(t), t \geq 0\}$ es una CMTC con espacio de estados:

$$S = \{ (0, 0), (0, 1), (0, 2), (1, 0), (1, 1), (1, 2), (2, 0), (2, 1), (2,2) \}$$

En esta situación, el avión sigue funcionando en el subconjunto de estados $S = \{ (1, 1), (1, 2), (2, 1), (2,2) \}$. 

Vamos a asumir (aunque no es real) que el sistema sigue funcionando, incluso cuando hay posibilidad de un accidente, hasta que llegamos al estado $(0, 0)$. Dado que no hay reparación posible, la matriz de tasas entre estados del sistema viene dada por

$$R = 
\begin{pmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\lambda & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 2\lambda & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\lambda & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & \lambda & 0 & \lambda & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & \lambda & 0 & 2\lambda & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 2\lambda & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 2\lambda & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 2\lambda & 0 & 2\lambda & 0
\end{pmatrix} $$

y el diagrama que representa el sistema resulta:

\begin{figure}

{\centering \includegraphics[width=0.95\linewidth]{04-CMTC_files/figure-latex/05-022-1} 

}

\caption{Diagrama de tasas para el sistema motores de aviones}(\#fig:05-022)
\end{figure}

Para facilitar el algoritmo de simulación en esta situación, vamos a asignar un código a cada uno de los posibles estados del sistema:

$$S = \{ 1 = (0, 0), 2 = (0, 1), 3 = (0, 2), 4 = (1, 0), 5 = (1, 1), 6 = (1, 2), 7 = (2, 0), 8 = (2, 1), 9 = (2,2) \}$$ 

y asumimos que la media del tiempo hasta que un motor falla cuando está encendido es de 20 horas, es decir, $\lambda = 1/20$. A continuación hemos construido una función para simular el sistema descrito:


```r
TSIM_aviones <- function(tasa)
{
  # Parámetros de la función
  # =========================
  # tasa: tasa de fallo de un motor
  
  # Valores fijos del sistema
  # codigos de motores funcionando obviando el estado inicial
  codigo <- 1:8
  # motores con fallo de acuerdo al código
  motoresOFF <- c(4, 3, 2, 3, 2, 1, 2, 1)
  
  # inicialización de parámetros del sistema 
  tiempo =  estado = fallos = vector()
  i<-1
  # Primer fallo
  # Posibles estados de salto
  saltos = c(codigo[6], codigo[8])
  # tiempos asociados a cada fallo
  simula = rexp(2, 2*tasa)
  # Seleccionamos el salto
  posicion = which.min(simula)
  if(posicion == 1)
  {
    estado[i] <- saltos[posicion]
    tiempo[i] <- simula[posicion] 
    fallos[i] <- motoresOFF[saltos[posicion]]
    # nuevo salto
    i <- i + 1
    saltos = c(codigo[3], codigo[5])
    simula = c(rexp(1, tasa), rexp(1, 2*tasa))
    posicion = which.min(simula)
    if(posicion == 1)
    {
      estado[i] <- saltos[posicion]
      tiempo[i] <- simula[posicion] 
      fallos[i] <- motoresOFF[saltos[posicion]]
      # dos últimos saltos
      estado[i+1] <- 2; estado[i+2] <- 1
      tiempo[i+1] <- rexp(1, 2*tasa); tiempo[i+2] <- rexp(1, tasa)
      fallos[i+1] <- motoresOFF[estado[i+1]]; fallos[i+2] <- motoresOFF[estado[i+2]]
    }
    else
    {
      estado[i] <- saltos[posicion]
      tiempo[i] <- simula[posicion] 
      fallos[i] <- motoresOFF[saltos[posicion]]  
      # dos últimos saltos
      estado[i+1] <- 2; estado[i+2] <- 1
      tiempo[i+1] <- rexp(1, tasa); tiempo[i+2] <- rexp(1, tasa)
      fallos[i+1] <- motoresOFF[estado[i+1]]; fallos[i+2] <- motoresOFF[estado[i+2]]
    }
  }
  else
  {
    estado[i] <- saltos[posicion]
    tiempo[i] <- simula[posicion] 
    fallos[i] <- motoresOFF[saltos[posicion]]
    # nuevo salto
    i <- i + 1
    saltos = c(codigo[7], codigo[5])
    simula = c(rexp(1, tasa), rexp(1, 2*tasa))
    posicion = which.min(simula)
    if(posicion == 1)
    {
      estado[i] <- saltos[posicion]
      tiempo[i] <- simula[posicion] 
      fallos[i] <- motoresOFF[saltos[posicion]]
      # dos últimos saltos
      estado[i+1] <- 2; estado[i+2] <- 1
      tiempo[i+1] <- rexp(1, 2*tasa); tiempo[i+2] <- rexp(1, tasa)
      fallos[i+1] <- motoresOFF[estado[i+1]]; fallos[i+2] <- motoresOFF[estado[i+2]]
    }
    else
    {
      estado[i] <- saltos[posicion]
      tiempo[i] <- simula[posicion] 
      fallos[i] <- motoresOFF[saltos[posicion]]  
      # dos últimos saltos
      estado[i+1] <- 2; estado[i+2] <- 1
      tiempo[i+1] <- rexp(1, tasa); tiempo[i+2] <- rexp(1, tasa)
      fallos[i+1] <- motoresOFF[estado[i+1]]; fallos[i+2] <- motoresOFF[estado[i+2]]
    }    
  }

  # Devolvemos resultados del sistema 
  res <- tibble(estado, tiempo, fallos)
  return(res)
}
```

Veamos una simulación del sistema:


```r
set.seed(12)
tasa <- 1/20
sistema <- TSIM_aviones(tasa)
sistema
```

```
## # A tibble: 4 x 3
##   estado tiempo fallos
##    <dbl>  <dbl>  <dbl>
## 1      8   6.36      1
## 2      7   2.35      2
## 3      2  18.2       3
## 4      1   5.67      4
```

El resultado obtenido es el estado en cada paso del algoritmo hasta llegar a la parada total, los tiempos para alcanzar cada estado, y el número de motores con fallo. 

Podemos operar los tiempos para saber cuándo tendremos más de dos motores con fallo (últimas dos filas de la simulación). 

Si el vuelo que acabamos de empezar es de $x$ horas ¿cómo podemos estimar la probabilidad de no poder terminar el vuelo de forma segura? Para responder esta pregunta podremos calcular la proporción de tiempo que corresponde con un fallo leve (1 o 2 motores con fallo) con respecto a las $x$ horas de duración del vuelo.

Calculamos a continuación el tiempo de fallo del sistema y evaluamos la probabilidad para diferentes tiempos de duración de vuelo.


```r
# Tiempo de fallo
Tfallo <- round(sum(sistema$tiempo[sistema$fallos <= 2]), 2)
# Duración del vuelo
td <- seq(0.1, 12, by = 0.1)
prob <- vector()
# Probabilidad viaje seguro
for(i in 1:length(td))
{
  prob[i] <- ifelse(Tfallo >= td[i], 1, round(Tfallo/td[i], 3))
}
# Obtenemos el valor mínimo de tiempo para asegurar un viaje seguro
limite <- min(td[which(prob != 1)])
limite
```

```
## [1] 8.8
```

Cualquier viaje inferior a 8.8 horas será un viaje seguro, mientras que si el tiempo es superior, la probabilidad de no tener un viaje seguro aumenta.

Para estudiar la estabilidad de dichas probabilidades realizamos 1000 simulaciones del sistema y estimamos las probabilidades mediante estimadores Monte-Carlo de los tiempos de fallo.


```r
nsim <- 1000
Tfallo <- vector()
# Repetición del sistema y calculo de tiempo 
for(i in 1:nsim)
{
  sistema <- TSIM_aviones(tasa)
  Tfallo[i] <- round(sum(sistema$tiempo[sistema$fallos <= 2]), 2)
}
# Estimador Monte-Carlo
estimMC<- mean(Tfallo)
# Duración del vuelo
td <- seq(0.1, 20, by = 0.1)
prob <- vector()
# Probabilidad viaje seguro
for(i in 1:length(td))
{
  prob[i] <- ifelse(estimMC >= td[i], 1, round(estimMC/td[i], 3))
}
# Obtenemos el valor mínimo de tiempo para asegurar un viaje seguro
limite <- min(td[which(prob != 1)])
limite
```

```
## [1] 11.9
```

Cualquier viaje inferior a 11.9 horas será un viaje seguro, mientras que si el tiempo es superior, la probabilidad de no tener un viaje seguro aumenta. Podemos representar el gráfico de probabilidad de un viaje seguro:

\begin{figure}

{\centering \includegraphics[width=0.95\linewidth]{04-CMTC_files/figure-latex/05-027-1} 

}

\caption{Probabilidad de un viaje seguro}(\#fig:05-027)
\end{figure}

## Procesos de nacimiento y muerte {#CMTCD}

Los procesos de nacimiento y muerte que estudiamos en este apartado juegan un papel muy importante dentro de las CMTC, ya que son de uso habitual en muchas aplicaciones prácticas que veremos de ahora en adelante.

::: {.yellowbox data-latex=""}
::: {#cmtc002 .definition}
Una CMTC $\{X_t; t \geq 0\}$ con espacio de estados $S = \{0, 1, 2,...,N\}$ y matriz de tasas dada por:

$$R = 
\begin{pmatrix}
0 & \lambda_0 & 0 & 0 & \ldots & 0 & 0 \\
\mu_1 & 0 & \lambda_1 & 0 & \ldots & 0 & 0 \\
0 & \mu_2 & 0 & \lambda_2 & \ldots & 0 & 0 \\
\vdots & \vdots & \vdots & \vdots & \ldots & \vdots & \vdots \\
0 & 0 & 0 & 0 & \ldots & 0 & \lambda_{N-1} \\
0 & 0 & 0 & 0 & \ldots &\mu_N & 0
\end{pmatrix} $$

se denomina proceso finito de nacimiento y muerte donde los $\lambda_i$ se denominan **parámetros de nacimiento** (transición del estado $i$ al $i+1$) y los $\mu_i$ se denominan **parámetros de muerte** (transición del estado $i$ al $i-1)$, donde por conveniencia se asume que $\lambda_K = 0$ y $\mu_0 = 0$ indicando que no hay nacimientos en el estado $K$ y que no hay muertes en el estado $0$.
:::
:::

En esta situación el proceso permanece una cantidad de tiempo $exp(\lambda_i + \mu_i)$ en el estado $i$ y después salta al estado $i+1$ con probabilidad $\lambda_i/(\lambda_i + \mu_i)$, o al estado $i-1$ con probabilidad $\mu_i/(\lambda_i + \mu_i)$. Además, la matriz generadora del proceso viene dada por:

$$R = 
\begin{pmatrix}
- \lambda_0& \lambda_0 & 0 & 0 & \ldots & 0 & 0 \\
\mu_1 & -(\mu_1 + \lambda_1) & \lambda_1 & 0 & \ldots & 0 & 0 \\
0 & \mu_2 & -(\mu_2 + \lambda_2) & \lambda_2 & \ldots & 0 & 0 \\
\vdots & \vdots & \vdots & \vdots & \ldots & \vdots & \vdots \\
0 & 0 & 0 & 0 & \ldots & -(\mu_{k-1} + \lambda_{k-1}) & \lambda_{k-1} \\
0 & 0 & 0 & 0 & \ldots & \mu_k & -\mu_k
\end{pmatrix} $$

A continuación presentamos diferentes ejemplos de aplicación de los procesos de nacimiento y muerte.

### Colas de espera de capacidad finita con un servidor

Aunque en la unidad siguiente estudiaremos con mucho más detalle los diferentes sistemas de colas de espera, vamos a presentar aquí el modelo más sencillo de colas, para comprobar que se puede modelar como un proceso de nacimiento y muerte.

Imaginemos que tenemos un cajero bancario al que los clientes acuden de acuerdo a Proceso de Poisson de parámetro $\lambda$, es decir que las llegadas son aleatorias y se distribuyen según una $Exp(\lambda)$. Si hay $K-1$ clientes haciendo cola para ser atendidos, un cliente nuevo tomará la opción de buscar otro cajero, es decir, el sistema tiene capacidad $K$ (1 cliente atendido y $K-1$ en la cola de espera). Además, el tiempo de servicio del cajero tiene una distribución $Exp(\mu)$.

En esta situación la variable $X(t)$ que indica el número de sujetos en el sistema en el instante $t$ es una CMTC denominada cola $M/M/1/K$, donde $M$ hace referencia a los tiempos de llegada y servicio exponenciales, el $1$ hace referencia a la capacidad del servicio (sólo un cliente puede ser atendido en un instante dado), $K$ es el tamaño del sistema (o aforo total), y $S = \{0, 1, 2,...K \}$ el espacio de estados. Este tipo de sistemas son procesos de nacimiento y muerte con:

\begin{eqnarray*}
\lambda_i &=& \lambda, \quad 0 \leq i \leq K-1, \\
\mu_i &=& \mu, \quad 0 \leq i \leq K,
\end{eqnarray*}

donde el "nacimiento" y la "muerte" hacen referencia a la llegada al cajero y la salida del cajero respectivamente.

Veamos la descripción del sistema:

-   En el estado $X_t=0$ el sistema está vacío y el único evento que puede ocurrir es una llegada, que se produce después de un tiempo $Exp(\lambda)$, provocando una transición al estado $X_t=1$. En este caso $r_{01} = \lambda$.

-   En el estado $X_t=i$ ($1 \leq i \leq K-1$), tenemos dos posibilidades en el sistema: una llegada o una salida. En el primer caso tenemos $r_{i, i+1} = \lambda$, mientras que en el segundo tenemos $r_{i, i-1} = \mu$.

-   En el estado $X_t=K$ sólo se puede producir una salida de forma que $r_{K, K-1} = \mu$.

Por tanto, la matriz de tasas correspondiente a este proceso viene dada por:

$$R = 
\begin{pmatrix}
0 & \lambda & 0 & 0 & \ldots & 0 & 0 \\
\mu & 0 & \lambda & 0 & \ldots & 0 & 0 \\
0 & \mu & 0 & \lambda & \ldots & 0 & 0 \\
\vdots & \vdots & \vdots & \vdots & \ldots & \vdots & \vdots \\
0 & 0 & 0 & 0 & \ldots & 0 & \lambda \\
0 & 0 & 0 & 0 & \ldots &\mu & 0
\end{pmatrix} $$

El diagrama de tasas para una cola $M/M/1/4$, es decir con una capacidad de 4 usuarios en el sistema (1 atendido y tres en espera) es:

\begin{figure}

{\centering \includegraphics[width=0.95\linewidth]{04-CMTC_files/figure-latex/05-028-1} 

}

\caption{Diagrama de tasas para cola M/M/1/4.}(\#fig:05-028)
\end{figure}

Este sistema puede implementarse fácilmente en `simmer` si entendemos como "resource" al servicio del cajero y como "generator" la llegada al cajero. Escribimos el algoritmo de forma general para cualquier valor de $K$.


```r
# Sistema
#################################################
cola.MM1K <- function(t, lambda, mu, servidores, usuarios)
{
  # lambda: tasa de llegadas
  # mu: tasa de servicio
  # servidores: número de servidores
  # usuarios: capacidad total del sistema
  cola <- usuarios - servidores
  
  servicio <- trajectory() %>%
    # empieza a ser atendido
    seize("atendiendo", amount = 1) %>%   
    # durante un tiempo exp(mu)
    timeout(function() rexp(1, mu)) %>% 
    # termina el servicio de atención
    release("atendiendo", amount = 1)               

  # Configuración del sistema 
  #################################################
  simmer() %>%
    # se crean los recursos (servidores) y se dimensiona la cola
    add_resource("atendiendo", capacity = servidores, queue_size = cola) %>%    
    # se generan las llegadas de clientes según Exp(lambda)
    add_generator("llegada", servicio, function() rexp(1, lambda)) %>% 
    run(until = t)     
}
```

Imaginemos que por la mañanas (de 8 a 15) las llegadas de clientes se producen con una tasa de 15 clientes/hora, mientras que el tiempo medio que el cliente permanece en el cajero es de 6 minutos. Expresada en minutos tendríamos una tasa de llegadas $\lambda = 15/60$, y una tasa de servicio $\mu = 1/6$. Se ha observado además que cuando la cola de espera es de tres clientes nadie más espera para hacer cola ($K = 4$). Analizamos el sistema de forma básica para una mañana cualquiera, es decir para un periodo de 7 horas (420 minutos).


```r
set.seed(12)
### Simulación del sistema
t=420; 
lambda=15/60; mu=1/6
servidores=1; usuarios=4
cajero <- cola.MM1K(t, lambda, mu, servidores, usuarios)
### Salidas del sistema
cajero.df.res <- get_mon_resources(cajero)  # recursos (servidores)
cajero.df.arr <- get_mon_arrivals(cajero)   # llegadas (clientes)
```

Veamos con un poco de detalle las salidas que proporciona el sistema. Comenzamos con el proceso de llegadas al sistema viendo las primeras 10 llegadas al sistema:


```r
head(cajero.df.arr, n = 10)
```

```
##         name start_time  end_time activity_time finished replication
## 1   llegada0   8.756865  9.461164      0.704299     TRUE           1
## 2   llegada1  11.299066 22.198513     10.899446     TRUE           1
## 3   llegada2  22.728061 46.307530     23.579469     TRUE           1
## 4   llegada7  53.191599 53.191599      0.000000    FALSE           1
## 5   llegada3  23.861384 53.496376      7.188846     TRUE           1
## 6   llegada9  57.528321 57.528321      0.000000    FALSE           1
## 7   llegada4  40.759227 59.558394      6.062018     TRUE           1
## 8  llegada11  68.130173 68.130173      0.000000    FALSE           1
## 9  llegada12  74.418248 74.418248      0.000000    FALSE           1
## 10  llegada5  45.814971 75.428935     15.870541     TRUE           1
```

En la columna `name` tenemos el identificador de la llegada al sistema (cajero), en las columnas `start_time` y `end_time` tenemos el instante de tiempo en el que llega y sale del cajero. La columna `activity_time` muestra el tiempo de servicio en el cajero (durante el que es atendido el cliente); un valor 0 identifica a un cliente que no ha sido atendido porque sobrepasó la capacidad del sistema (no esperó al ver la cola y se marchó). Estos coinciden con los valores `FALSE` de la columna `finished`, que identifica a los clientes que no han podido ser atendidos. Con estas salidas resulta muy fácil calcular el tiempo de servicio del sistema así como el porcentaje de clientes rechazados, y el tiempo esperando en la cola.


```r
# llegadas al sistema: número de clientes que han pasado por el cajero
nsis <- nrow(cajero.df.arr);nsis
```

```
## [1] 108
```

```r
# tiempo total de servicio
tserver <- sum(cajero.df.arr$activity_time);tserver
```

```
## [1] 407.9837
```

```r
# proporción de tiempo que el sistema está ocupado (atendiendo o en espera)
round(100*tserver/t,2)
```

```
## [1] 97.14
```

```r
# porcentaje de clientes que se marcharon sin ser atendidos
rechazados <- sum(cajero.df.arr$finished == FALSE)
round(100*rechazados/nsis,2)
```

```
## [1] 48.15
```

```r
# Tiempos de espera en cola para ser atendido
tespera <- cajero.df.arr$end_time - cajero.df.arr$start_time - cajero.df.arr$activity_time
# tiempo medio de espera en cola y desv.típica
mean(tespera);sd(tespera)
```

```
## [1] 8.280251
```

```
## [1] 10.53434
```

Podemos estudiar el comportamiento del sistema, y en particular de la cola, también en términos de los servidores.


```r
head(cajero.df.res, n = 10)
```

```
##      resource      time server queue capacity queue_size system limit replication
## 1  atendiendo  8.756865      1     0        1          3      1     4           1
## 2  atendiendo  9.461164      0     0        1          3      0     4           1
## 3  atendiendo 11.299066      1     0        1          3      1     4           1
## 4  atendiendo 22.198513      0     0        1          3      0     4           1
## 5  atendiendo 22.728061      1     0        1          3      1     4           1
## 6  atendiendo 23.861384      1     1        1          3      2     4           1
## 7  atendiendo 40.759227      1     2        1          3      3     4           1
## 8  atendiendo 45.814971      1     3        1          3      4     4           1
## 9  atendiendo 46.307530      1     2        1          3      3     4           1
## 10 atendiendo 48.326016      1     3        1          3      4     4           1
```

En este caso las columnas nos informan sobre el instante de tiempo (`time`) en el que se produce alguna actividad en el sistema, la columna `server` indica el número de servidores ocupados atendiendo a algún cliente, `queue` el número de clientes en la cola de espera, `capacity` la capacidad de servicio del sistema, `queue_size` el tamaño máximo de la cola, `system` el número de clientes en el sistema, `limit` la capacidad total de sistema, y finalmente `replication` da un indicador de la replicación de las simulaciones.

Con estas salidas podemos describir fácilmente el comportamiento de la cola del sistema ya que podemos estudiar el número de clientes en cola a lo largo del tiempo.


```r
# Evolución del tamaño de la cola
ggplot(cajero.df.res, aes(time, queue)) +
  geom_step() +
  scale_x_continuous(breaks = seq(0, 420, 60)) + 
  labs(x = "Tiempo (en minutos)", y = "Tamaño de la cola")
```



\begin{center}\includegraphics[width=0.95\linewidth]{04-CMTC_files/figure-latex/05-034-1} \end{center}

```r
# Porcentaje de clientes en cola
ggplot(cajero.df.res, aes(x = queue)) + 
  geom_bar(aes(y = ..prop.. , group = 1)) + 
  scale_y_continuous(labels = scales::percent) + 
  labs(x = "Clientes en la cola", y = "Porcentaje")
```



\begin{center}\includegraphics[width=0.95\linewidth]{04-CMTC_files/figure-latex/05-034-2} \end{center}

Simulamos ahora el sistema en 500 ocasiones para inferir con mayor precisión el comportamiento del sistema.


```r
# Réplicas del sistema
nreplicas=500
t=420; 
lambda=15/60; mu=1/6
servidores=1; usuarios=4

envs <- replicate(nreplicas,cola.MM1K(t, lambda, mu, servidores, usuarios))
# almacenamos análisis de llegadas del sistema
simarrivals<-as_tibble(get_mon_arrivals(envs))

salida<-simarrivals %>% 
  group_by(replication) %>% 
  # Resumen de las simualciones
  summarise(clientes = n(), 
            tiemposervicio = sum(activity_time),
            proptiemposervicio = round(tiemposervicio/420, 2),
            rechazados = clientes - sum(finished),
            proprechazados = round(rechazados/clientes,2)) %>% 
  summarise(media_clientes = mean(clientes),
            media_tserver = mean(tiemposervicio),
            media_ptserver = 100*mean(proptiemposervicio),
            media_rechazados = mean(rechazados),
            media_prechazados = 100*mean(proprechazados)
            )
salida
```

```
## # A tibble: 1 x 5
##   media_clientes media_tserver media_ptserver media_rechazados media_prechazados
##            <dbl>         <dbl>          <dbl>            <dbl>             <dbl>
## 1           102.          377.           89.8             37.9              36.7
```

> Para comentar: ¿Qué conclusiones extraemos del análisis realizado? ¿Cómo valorarías la ocupación del sistema? ¿Qué ocurriría si añadimos un nuevo cajero y ampliamos la capacidad del sistema a 8 clientes?

### Mantenimiento de máquinas {#excmtc007}

El problema del mantenimiento de máquinas es muy habitual dentro de las CMTC. Supongamos que disponemos de $N$ máquinas que funcionan durante 24 horas seguidas y $M$ personas que pueden repararlas ($M \leq N$). Las máquinas son idénticas, y los tiempos de vida de las máquinas (reparaciones o mantenimiento) son variables aleatorias independientes $Exp(\mu)$. Cuando las máquinas fallan, son reparadas por orden de fallo (la primera que falla es la primera en ser reparada) por los $M$ reparadores. Cada máquina averiada necesita una y sólo una persona para repararla, y los tiempos de reparación se distribuyen como una $Exp(\lambda)$; una vez reparada, la máquina continúa comportándose como una máquina nueva. Si $X(t)$ el número de máquinas que funcionan en el momento $t$, el proceso $\{X(t), t \geq 0\}$ es un proceso de nacimiento (avería) y muerte (reparación) con parámetros:

$$\lambda_i = \lambda \cdot min(N-i, M), \quad 0 \leq i \leq N,$$

$$\mu_i = \mu \cdot i, \quad 0 \leq i \leq N.$$
::: example

Imaginemos un problema sencillo de mantenimiento de máquinas, que se puede generalizar fácilmente, en el que tenemos 4 máquinas y 2 reparadores, de forma que el espacio de estados (número de máquinas en funcionamiento) viene dado por $S = \{0, 1, 2, 3, 4\}$. En este ejemplo vamos a ver cómo los parámetros de nacimiento y muerte corresponden con los que acabamos de definir. 

:::

Descripción del sistema:

-   En el estado $X(t)=0$, todas las máquinas están estropeadas, dos se encuentran en reparación (puesto que hay dos reparadores) y dos esperando a ser reparadas, Los tiempos de reparación son iid $Exp(\lambda)$ y un vez se complete cualquiera de las dos repaciones el sistema cambiará al estado 1. De esta forma, $r_{01} = \lambda_0 = \lambda + \lambda = 2\lambda$.

-   En el estado $X(t)=1$, una máquina está en funcionamiento, hay dos en reparación y otra está en espera. Cuando una de las dos máquinas es reparada pasamos al estado 2. De esta forma, $r_{12} = \lambda_1 = 2\lambda$. Además, en este estado la máquina que está funcionando puede fallar después de mantenerse en funcionamiento un tiempo $Exp(\mu)$ volviendo al estado 0, de forma que, $r_{10} = \mu_1 = \mu$.

-   En el estado $X(t)=2$, con razonamientos similares, tendremos que $r_{23} = \lambda_2 = 2\lambda$ y $r_{21} = \mu_2 = 2\mu$.

-   Para el resto de estados tendremos que $r_{34} = \lambda_3 = \lambda$, $r_{32} = mu_3 = 3\mu$, y $r_{43} = \mu_4 = 4\mu$.

En esta situación el diagrama del proceso viene dado por:

\begin{figure}

{\centering \includegraphics[width=0.95\linewidth]{04-CMTC_files/figure-latex/05-036-1} 

}

\caption{Diagrama de tasas para el mantenimiento de máquinas}(\#fig:05-036)
\end{figure}

Este sistema se puede modelizar fácilmente en `simmer` sin más que fijar las tasas correspondientes, el número de máquinas disponibles, y el número de operarios. Supongamos que los tiempos de vida de las máquinas son variables aleatorias exponenciales con media de 3 días, mientras que los tiempos de reparación son variables exponenciales con media de 2 horas. Expresando todo en horas, tendríamos una tasa de reparación de $\mu = 1/2$, y una tasa de llegadas $\lambda = 1/72$.

De nuevo planteamos una función que nos permita cambiar fácilmente los parámetros del sistema si fuera necesario. La empresa está interesada en:

-   ¿Cuántas máquinas estarán en funcionamiento después de 3 días?
-   ¿Durante qué porcentaje del tiempo las cuatro máquinas están funcionando?


```r
# Sistema
#################################################
mantenimiento <- function(t, lambda, mu, capacidad)
{
  # lambda: tasa de reparación
  # mu: tasa de vida de la máquina
  # capacidad: reparadores
  # K: máquinas
  
  # Distribuciones de tiempos
  vida <- function() rexp(1, lambda)
  reparacion <- function() rexp(1, mu)
  
  # Trayectoria 
  maquina <- trajectory() %>%
    seize("funcionando") %>%
    timeout(vida) %>%
    release("funcionando") %>%
    seize("reparando") %>%
    timeout(reparacion) %>%
    release("reparando") %>%    
    rollback(6)

  # Configuración del sistema 
  simmer() %>%
    add_resource("funcionando", capacity = Inf) %>%
    add_resource("reparando", capacity = capacidad, queue_size = Inf) %>%
    add_generator("sistema", maquina, at(0, 0, 0, 0)) %>%
    run(until = t)     
}
```

Simulamos el sistema y analizamos un poco la salida. Dado que en este sistema no hay llegadas solo podemos estudiar los recursos.


```r
t=72 # 3 días
lambda=1/72; mu=1/2
capacidad=2 # nº reparadores
### Simulación del sistema
maquinas <- mantenimiento(t, lambda,mu, capacidad)
### Salidas del sistema
maquinas.df.res <- get_mon_resources(maquinas)
head(maquinas.df.res, 10)
```

```
##       resource     time server queue capacity queue_size system limit replication
## 1  funcionando  0.00000      1     0      Inf        Inf      1   Inf           1
## 2  funcionando  0.00000      2     0      Inf        Inf      2   Inf           1
## 3  funcionando  0.00000      3     0      Inf        Inf      3   Inf           1
## 4  funcionando  0.00000      4     0      Inf        Inf      4   Inf           1
## 5  funcionando 39.64840      3     0      Inf        Inf      3   Inf           1
## 6    reparando 39.64840      1     0        2        Inf      1   Inf           1
## 7    reparando 40.91384      0     0        2        Inf      0   Inf           1
## 8  funcionando 40.91384      4     0      Inf        Inf      4   Inf           1
## 9  funcionando 63.84362      3     0      Inf        Inf      3   Inf           1
## 10   reparando 63.84362      1     0        2        Inf      1   Inf           1
```

Respondemos en primer lugar al número de máquinas en funcionamiento a los tres días


```r
# última iteración del sistema
tail(maquinas.df.res, 1)
```

```
##       resource     time server queue capacity queue_size system limit replication
## 12 funcionando 65.62617      4     0      Inf        Inf      4   Inf           1
```

A las 72 horas el número de máquinas funcionando se corresponde con el valor de `server`. Para conocer el tiempo que las cuatro máquinas han estado funcionando debemos manipular los resultados obtenidos en el proceso de simulación.


```r
### Seleccionamos todos los recursos cuando las máquinas están funcionando
maquinas.df.sel <- maquinas.df.res %>%
  subset(resource == "funcionando")
### Calculamos los tiempos de funcionamiento
deltas_4 <- diff(maquinas.df.sel$time)
### Seleccionamos cuando las 4 máquinas están activas
both_working_4 <- which(maquinas.df.sel$system == 4)
t_both_working_4 <- deltas_4[both_working_4]
### Caculamos la proporción de tiempo
res<-sum(t_both_working_4, na.rm=TRUE) / max(maquinas.df.sel$time)
```

La proporción de tiempo en que las cuatro máquinas están funcionando simultáneamente es 95.36. Estudiamos la estabilidad del sistema realizando 500 simulaciones del sistema y estimando las cantidades de interés para la empresa.


```r
# Réplicas del sistema
replicas <- 500
envs <- lapply(1:replicas, function(i){
  maquinas <- mantenimiento(72, 1/2, 1/72, 2)
})

# almacenamos análisis de llegadas del sistema
simresources<-as_tibble(get_mon_resources(envs))
## funciones para calcular cantidades de interés
maquinasON <- vector()
propTimeON <- vector()
for (i in 1:500)
{
  datos <- simresources[simresources$replication == i,]
  # máquinas en funcionamiento
  maquinasON[i] <- tail(datos, 1)$server
  # Proporción de tiempo
  maquinas.df.sel <- datos %>%
  subset(resource == "funcionando")
  ### Calculamos los tiempos de funcionamiento
  deltas_4 <- diff(maquinas.df.sel$time)
  ### Seleccionamos cuando las 4 máquinas están activas
  both_working_4 <- which(maquinas.df.sel$system == 4)
  t_both_working_4 <- deltas_4[both_working_4]
  ### Caculamos la proporción de tiempo
  propTimeON[i]<-round(100*sum(t_both_working_4, na.rm=TRUE) / max(maquinas.df.sel$time), 2)
}
```

Tenemos entonces que el número de máquinas en funcionamiento es 1.97 y el tiempo de funcionamiento de las cuatro máquinas estimado es 3.58 para un periodo de 72 horas cuando al inicio todas las máquinas están paradas.

### Central telefónica

Una centralita telefónica puede atender $K$ llamadas a la vez en un momento dado. Las llamadas llegan según un proceso de Poisson con tasa $\lambda$. Si la centralita ya está atendiendo $K$ llamadas cuando llega una nueva llamada, ésta se pierde. Si una llamada es aceptada, dura un tiempo $Exp(\mu)$ y luego termina. Todas las llamadas son independientes entre sí. Sea $X(t)$ el número de llamadas que la centralita gestiona en el momento $t$.

El proceso $\{X(t); t \geq 0\}$ es una CMTC con espacio de estados $S = \{0, 1, 2,...,K\}$ de forma que:

-   En el estado $i$, con $0 \leq i \leq K-1$ la llegada de una llamada desencadena una transición al estado $i+1$ con tasa $\lambda$ ($r_{i i+1} = \lambda$), mientras que en el estado $K$ no se pueden recibir llamadas.

-   En el estado $i$, con $1 \leq i \leq K$ cualquiera de las llamadas $i$ puede completarse y desancedar una transición al estado $i-1$. La tasa de transición es $r_{i i-1} = i\mu$. En el estado 0 no hay salidas.

El sistema $\{X(t); t \geq 0\}$ es un proceso de nacimiento y muerte con:

$$\lambda_i = \lambda, \quad 0 \leq i \leq K-1$$ $$\mu_i = i\mu, \quad 0 \leq i \leq K$$ que como veremos más adelante se denomina cola $M/M/K/K$, es decir, llegadas y servicios exponenciales con $K$ servidores y de capacidad $K$.

La función de simmer para estudiar este sistema viene dada por:


```r
# Sistema
#################################################
cola.MMKK <- function(t, lambda, mu, servidores, usuarios)
{
  # lambda: tasa de llegadas
  # mu: tasa de servicio
  # servidores: número de servidores
  # usuarios: capacidad total del sistema
  cola <- usuarios - servidores
  
  servicio <- trajectory() %>%
    seize("atendiendo", amount = 1) %>%              
    timeout(function() rexp(1, mu)) %>%   
    release("atendiendo", amount = 1)               

  # Configuración del sistema 
  #################################################
  simmer() %>%
    add_resource("atendiendo", capacity = servidores, queue_size = cola) %>%           
    add_generator("llegada", servicio, function() rexp(1, lambda)) %>% 
    run(until = t)     
}
```

### Call Center

El sistema de reservas telefónicas de una aerolínea es un "call center" formado por $s$ empleados de reservas llamados agentes. Una llamada entrante para una reserva es atendida por un agente si hay uno disponible; de lo contrario, la persona que llama es puesta en espera. El sistema puede poner en espera a un máximo de $H$ personas. Cuando un agente está disponible, las llamadas en espera se atienden por orden de llegada. Cuando todos los agentes están ocupados y hay $H$ llamadas en espera, cualquier llamada adicional recibe una señal de ocupado y se pierden permanentemente. Sea $X(t)$ la varaible aleatoria que registra el número de llamadas en el sistema, las atendidas por los agentes más las que están en espera, en el momento $t$ . Si las llamadas recibidas se comportan como un $PP(\lambda)$ y los tiempos de procesamiento de las llamadas son variables aleatorias iid $Exp(\mu)$, el sistema $\{X(t); t \geq 0\}$ es una CMTC, con espacio de estados $S = \{0, 1, 2,...,K\}$ donde $K = s + H$.

Se puede demostrar fácilmente que este sistema es un proceso de nacimiento y muerte con tasas:

$$\lambda_i = \lambda, \quad 0 \leq i \leq K-1$$ $$\mu_i = min(i, s)\mu, \quad 0 \leq i \leq K$$ que en la terminologia habitual se denomina cola $M/M/s/K$. Para simular este proceso podemos utilizar la función del ejemplo anterior.

## Otros tipos de sistemas {#CMTCE}

Presentamos en este punto otros sistemas que no se corresponden con procesos de nacimiento y muerte, pero que son muy habituales en el mundo real.

### Gestión de inventarios

Una tienda minorista gestiona el inventario de un tipo de producto, que denominamos $P$, de la forma siguiente. Cuando el número de elementos de $P$ disminuye a un número fijo $l$, se hace un pedido al fabricante de $m$ repuestos de $P$. El pedido tarda un tiempo aleatorio en ser entregado al minorista. Si el inventario es como máximo $l$ cuando se entrega un pedido (incluido el pedido recién entregado), se realiza inmediatamente otro pedido de $m$ artículos. Supongamos que que los plazos de entrega son variables aleatorias iid $Exp(\lambda)$ y que la demanda se produce según un $PP(\mu)$. Las demandas que no pueden ser satisfechas inmediatamente se pierden.

Sea $X(t)$ el número de elementos de $P$ en stock en el momento $t$. Obsérvese que el número máximo de elemntos de $P$ en stock es $K = l + m$, lo que ocurre si el pedido se entrega antes de que se produzca la siguiente demanda. El espacio de estados es, pues, $S = \{0, 1, 2,...,K\}$. En el estado $0$, las demandas se pierden, y el stock salta a $m$ cuando se entrega el pedido pendiente actual (lo que ocurre a la tasa $\lambda$). Por tanto, tenemos $r_{0m} = \lambda$. En el estado $i$ $(1 \leq i \leq l)$ hay un pedido pendiente. El estado cambia a $i-1$ si se produce una demanda (lo que ocurre a la tasa $\mu$) y a $i + m$ si se entrega el pedido. Por lo tanto, tenemos $r_{i i+m} = \lambda$ y $r_{i i-1} = \mu$. Finalmente, si $X(t) = i$ ($l + 1 \leq i \ K$), no hay pedidos pendientes, y la única transición es de $i$ a $i- 1$, y eso ocurre cuando se produce una demanda. Por lo tanto, $r_{i i-1} = \mu$. El proceso $X(t), t \geq 0$ definidido de esta forma es una CMTC.

Consulta sobre modelo simmer para inventarios (<https://stackoverflow.com/questions/51680140/immediate-inventory-restock-in-r-simmer>)

### Proceso de fabricación

Un proceso de de fabricación sencilla consiste en una sola máquina que puede estar encendida o apagada. Si la máquina está encendida, produce artículos según un proceso de Poisson con tasa $\lambda$. La demanda de artículos llega según un $PP(\mu)$ La máquina se controla de la siguiente manera. Si el número de artículos en stock alcanza un número máximo $K$ (la capacidad de almacenamiento), la máquina se apaga. La máquina se enciende cuando el número de artículos en stock disminuye hasta un nivel preestablecido $l < K$. Si la variable aleatoria $X(t)$ nos indica el número de artículos en stock en el momento $t$, el proceso $X(t), t \geq 0$ no es una CMTC ya que no sabemos si la máquina está encendida o apagada si $l < X(t) < K$. Si se considera $Y(t)$ como el estado en el que se encuentra la máquina en el momento $t$, de forma que un $1$ indica encendido y un $0$ que está apagada, entonces el proceso $\{X(t), Y(t), t\geq 0\}$ es una CMTC con espacio de estados:

$$S = \{(i, 1), 0 \leq i < K\} \cup \{(i, 0), l < i \leq K\}$$

Hay que tener en cuenta que la máquina siempre está encendida si el número de elementos es $l$ o menos. Por lo tanto, no necesitamos los estados $\{(i, 0), 0 \leq i \leq l\}$. El análisis habitual de los eventos desencadenantes arroja las siguientes tasas de transición:

$$r_{(i, 1)(i+1, 1)} = \lambda, \quad 0 \leq i < K-1,$$

$$r_{(K-1, 1)(K, 0)} = \lambda, $$ $$r_{(i, 1)(i-1, 1)} = \mu, \quad 1\leq i\leq K-1,$$

$$r_{(i, 0)(i-1, 0)} = \mu, \quad l+1 < i\leq K,$$

$$r_{(l+1, 0)(l, 1)} = \mu.$$

## Probabilidades de transición {#CMTCF}

El aspecto funcdamental para estudiar el comportamiento de cualquier CMTC es la obtención y análisis de las probabilidades de transición entre los estados del procso a partir de la matriz de tasas obtenida en puntos anteriores. Aunque haremos un desarrollo teórico de este rporblema, veremos que es necesario un algoritmo de computación para obtener dichas probabilidades.

Sea $X(t), t \geq 0$ una CMTC con espacio de estados $S = \{1,2,...,N\}$ y con matriz de tasas $R = [r_{ij}]$. Si asumimos que la distribución de probabilidad en el estado incial, $X(0)$ es conocida, entonces tenemos que:

$$P(X(t) = j) = \sum_{i=1}^N P(X(t) = j | X_0 = i)P(X_0 = i), \quad 1 \leq j \leq N.$$ Es necesario obtener $P(X(t) = j | X_0 = i) = p_{ij}(t)$ para obtener la función de distribución de probabilidad de $X(t)$. Antes de ver como obtener dichas probabildiades introducimos la notación necesaria.

ya hemos visto antes que una CMTC permance un tiempo $Exp(r_i)$ en el estado $i$ con $r_i = \sum_{j=1}^N r_{ij}$ y, si $r_i > 0$ entonces pasamos al estado $j$ con probabilidad $p_{ij} = r_{ij}/r_i$. Asumiendo que existe un número finito $r$ que satisface:

$$r \geq max(r_i), \quad 1\leq i \leq N$$ podemos definir la matriz $\hat{P} = [\hat{p}_{ij}]$ como:

```{=tex}
\begin{equation}
\hat{p}_{ij} = 
\begin{cases} 
1-\frac{r_i/r} & \text{ si } i=j\\
r_{ij}/r & \text{ si } i \neq j
\end{cases}
\end{equation}
```
que es una matriz estocástica.

::: theorem
La matriz de tarnsición de probabilidades, $P$, de una CMTC viene dada por:

$$P(t) = \sum_{k=0}^{\infty} e^{-rt}\frac{(rt)^k}{k!} \hat{P}^k$$
:::

Esta forma de obtener $P$ se denomina **método de uniformización**, y proporciona un método numérico para obtener las probabilidades de transición deseadas, sin más que usar los $m$ primeros términos de la serie infinita definida. Para asegurar la convergencia de dichas probabilidades se usan como reglas habituales:

$$m \approx max\{rt + r\sqrt{rt}, 20\},$$

y

$$r = max(r_i), \quad 1\leq i \leq N,$$ aunque en algunos casos resulta más conveniente utilizar $r = sum_{i=1}^N r_i$. De hecho, veremos que ambos resultados son muy similares y podremos usar la suma en todos los casos.

::: {.silverbox data-latex=""}
Algoritmo para obtener $P$:

1.  Fijar $R$, $t$, y $0 < \epsilon < 1$. Por defecto fijamos $\epsilon = 0.00001$.
2.  Obtener r.
3.  Calcular $\hat{P}$.
4.  Calcular $A = \hat{P}$; $c = e^{rt}$; $B = e^{rt}I$; $sum = c$; $k=1.$
5.  Mientras que $sum < 1-\epsilon$, calcular:

$$c = c*(rt)/k; \quad B = B + cA; \quad A = A \hat{P}$$ $$sum = sum + c; \quad k = k + 1$$ Al finalizar la matriz $B$ es una aproximación de $P(t)$ con error inferior a $\epsilon$.
:::

En realidad en nuestro algoritmo añadiremos un parámetro extra para indicar como se debe calcular el valor de $r$, bien como el máximo los elementos por filas de $R$ o como la suma de todos ellos.


```r
matriz.prob.trans<- function(Rmat, ts, cal)
{
  # Algortimo de uniformización para obtener P(t)
  ################################################
  
  # Parámetros de la función
  # Rmat: matriz de tasas
  # ts: instante de tiempo
  # epsilon: error en la aproximación
  # cal: forms de obtener r con dos valores 1 = máximo, 2 = suma
  epsilon <- 1e-05
  # Paso 2. Calculo de r
  ris <- apply(Rmat, 1, sum)
  ifelse(cal == 1, rlimit <- max(ris), rlimit <- sum(Rmat))
  # Paso 3. Calculo de hat(P)
  hatP <- Rmat/rlimit
  diag(hatP) <- 1 - ris/rlimit
  # Paso 4. Calculo de matrices y vectores accesorios
  rts <- rlimit*ts
  A <- hatP
  c <- exp(-rts)
  B <- c*diag(nrow(Rmat))
  suma <- c
  k <- 1
  # Bucle simulación
  cota <- 1- epsilon
  while(suma < cota)
  {
    c <- c*rts/k
    B <- B + c*A
    A <- A%*%hatP
    suma <- suma + c
    k <- k + 1
  }
  return(round(B, 4))
}
```

Veamos un ejemplo donde la matriz de tasas viene dada por:


```r
R = matrix(c(0, 2, 3, 0, 4, 0, 2, 0, 0, 2, 0, 2, 1, 0, 3, 0), byrow = TRUE, ncol = 4)
```

Si estamos interesados en las probabilidades de transición entre estados cuando han transcurrido 2 unidades de tiempo podemos calcular (ambas versiones):


```r
Pmat1<-matriz.prob.trans(R, 2, 1); Pmat1
```

```
##        [,1]   [,2] [,3]   [,4]
## [1,] 0.2001 0.2001  0.4 0.1998
## [2,] 0.2002 0.2001  0.4 0.1997
## [3,] 0.1999 0.2000  0.4 0.2001
## [4,] 0.1998 0.1999  0.4 0.2003
```

```r
Pmat2<-matriz.prob.trans(R, 2, 2); Pmat2
```

```
##        [,1]   [,2] [,3]   [,4]
## [1,] 0.2001 0.2001  0.4 0.1998
## [2,] 0.2002 0.2001  0.4 0.1997
## [3,] 0.1999 0.2000  0.4 0.2001
## [4,] 0.1998 0.1999  0.4 0.2003
```

Ambas matrices proporciona un resultado prácticamente idéntico. Estas matrices nos permiten obtener las probabildiades de pasar del estado 1 a cualquiera de los otros estados en dos unidades de tiempo, sin más que tomar los elementos de la fila 1 de la matriz obtenida.


```r
Pmat1[1,]; Pmat2[1,]
```

```
## [1] 0.2001 0.2001 0.4000 0.1998
```

```
## [1] 0.2001 0.2001 0.4000 0.1998
```

¿Cómo interpretamos esas probabilidades?

Veamos ahora la aplicación de este algoritmo a alguno de los ejemplos con los que hemos ido trabajando en esta unidad.

::: example
Para los datos correspondientes al cajero bancario estamos interesados en conocer la probabilidad de que después de 50 minutos de funcionamiento el sistema este completamente ocupado (1 usuario atendido y tres en cola), cuando partímos de $0$ clientes en el sistema ($p_{04}(4)$). La matriz de tasas viene dada por:
:::


```r
estados <- c(0, 1, 2, 3, 4)
nestados <- length(estados)

R <- matrix(nrow = nestados, ncol = nestados, data = 0)
lambda <- 15/60
mu <- 1/6 

R[1,2] <- lambda 
R[2,1] <- mu 
R[2,3] <- lambda 
R[3,2] <- mu 
R[3,4] <- lambda 
R[4,3] <- mu 
R[4,5] <- lambda
R[5,4] <- mu
```

Obtenemos la distribución de probabilidad asociada al estado 4 para $t = 50$:


```r
# Matriz de probabilidades de transición
Pmat<-matriz.prob.trans(R, 50, 2)
Pmat
```

```
##        [,1]   [,2]   [,3]   [,4]   [,5]
## [1,] 0.0812 0.1190 0.1730 0.2528 0.3741
## [2,] 0.0793 0.1172 0.1722 0.2539 0.3775
## [3,] 0.0769 0.1148 0.1711 0.2553 0.3819
## [4,] 0.0749 0.1128 0.1702 0.2565 0.3856
## [5,] 0.0739 0.1118 0.1698 0.2571 0.3874
```

La probabilidad de interés es 0.3741 ($p_{15}(50)$) lo que demuestra que es factible que el sistema llegue al estado 4 partiendo del estado 0 después de 4 horas.

Aunque el cálculo teórico es muy preciso hay situaciones donde los sistemas reales con los que estamos trabajndo hacen bastante costoso obtener la matriz $R$, y resulta más sencillo tratar de aproximar las probabilidades de transición mediante simulación. Para ello basta con replicar el sistema de simulación un número los suficientemente grande ya aproximar mediante Monte-Carlo. En este caso deberíamos obtner el estado dle sistema después de 50 minutos y aproximar la probabilidad como el número de veces que se alcanza el estado de interés dividido por las réplicas realizadas. la precisión de esta aproximación depende en gran medida del número de réplicas.


```r
replicas <- 2500
envs <- lapply(1:replicas, function(i) {
    repcajero <- cola.MM1K(50, 15/60, 1/6, 1, 4)
})
# almacenamos análisis de recursos del sistema
simresource <- as_tibble(get_mon_resources(envs))
# Almacenamos el estado final de la cola en el último instante del sistema
salida <- simresource %>%
  group_by(replication) %>%
  summarise(estado = last(system))
# Estimamos la probabilidad
round(table(salida$estado)/replicas, 3)
```

```
## 
##     0     1     2     3     4 
## 0.089 0.122 0.162 0.251 0.376
```

Podemos ver que la aproximación obtenida es similar (hasta el segundo decimal) a la obtneida a partir de la matriz $R$ del proceso. Simulando podemos aproximar las cantidades de interés siempre que el sistema empieza desde el punto de interés.

*Para practicar la obtención de la matriz de probabilidades de transición puedes resolver los ejercicios B-1 a B-4 de la colección al final de la unidad.*

## Análisis de tiempos de ocupación {#CMTCG}

En esta sección, nos concentramos en el análisis de los tiempos de ocupación de un estado determinado en un intervalo de tiempo finito $[0, T]$, es decir, la duración esperada de tiempo que el sistema pasa en ese estado. Como ocurre con las probabildiades de trasnsición presentamos un algortimo para obtener las cantidades de interés a partir de la matriz de tasas, y veremos como la simulación del proceso nos puede ayudar a dar respuesta a las mismas cuestiones.

::: theorem
Si $P(t)$ es la matriz de transiciones de probabilidad del proceso $\{X(t), t \geq 0\}$ con espacio de estados $S = \{1, 2,...,N\}$, se define la cantidad $m_{ij}(T)$ como el tiempo de ocupación del estado $j$ hasta el tiempo $T$ partiendo del estado $i$ como:

$$m_{ij}(T) = \int_0^T p_{ij}(t)dt, \quad 1 \leq i, j \leq N.$$
:::

Cuando tenemos formas explicitas para cada uno de los elemntos de $P(t)$ (este es el caso para la mayoría de los sistemas de colas de espera) este probelma se puede resolver teóricamente, pero en la mayoria de ocasiones es necesario un algoritmo de computación para aproximar estas cantidades. A continuación se presenta el algoritmo necesario, pero antes veamos la aproximación mediante series de la matriz $M(T) = [m_{ij}(t)]$.

::: theorem
Si $Y$ es una variable aletoria Poisson de parámetro $r*t$ entonces:

$$M(T) = \frac{1}{r} \sum_{k = 0}^{\infty} P(Y > k) \hat{P}^k, \quad T \geq 0.$$
:::

::: {.silverbox data-latex=""}
Algoritmo para obtener $M(T)$:

1.  Fijar $R$, $T$, y $0 < \epsilon < 1$. Por defecto fijamos $\epsilon = 0.00001$.
2.  Obtener r.
3.  Calcular $\hat{P}$.
4.  Calcular $A = \hat{P}$; $k = 0$
5.  Calcular $yek = exp(-r*t)$, $ygk = 1 - yek$, $suma = ygk$
6.  Calcular $B = ygk * I$
7.  Mientras que $suma/r < T-\epsilon$, calcular:

$$k = k + 1; \quad yek = yek*(rT)/k; \quad ygk = ygk - yek$$ $$B = B + ygk*A; \quad A = A\hat{P}; \quad suma = suma + ygk$$ Al finalizar la matriz $B/r$ es una aproximación de $M(T)$ con error inferior a $\epsilon$.
:::

En realidad en nuestro algoritmo añadiremos un parámetro extra para indicar como se debe calcular el valor de $r$, bien como el máximo los elementos por filas de $R$ o como la suma de todos ellos.


```r
tiempos.ocupacion<- function(Rmat, Ts, cal)
{
  # Algortimo de uniformización para obtener M(T)
  ################################################
  
  # Parámetros de la función
  # Rmat: matriz de tasas
  # ts: instante de tiempo
  # epsilon: error en la aproximación
  # cal: forms de obtener r con dos valores 1 = máximo, 2 = suma
  epsilon <- 1e-05
  # Paso 2. Calculo de r
  ris <- apply(Rmat, 1, sum)
  ifelse(cal == 1, rlimit <- max(ris), rlimit <- sum(Rmat))
  # Paso 3. Calculo de hat(P)
  hatP <- Rmat/rlimit
  diag(hatP) <- 1 - ris/rlimit
  # Paso 4. 
  k <- 0
  A <- hatP
  # Paso 5.
  yek <- exp(-1*rlimit*Ts)
  ygk <- 1 - yek
  suma <- ygk
  # Paso 6.
  B <- ygk*diag(nrow(Rmat))
  # Bucle simulación
  cota <- Ts- epsilon
  while(suma/rlimit < cota)
  {
    k <- k + 1
    yek <- yek*(rlimit*Ts)/k
    ygk <- ygk - yek
    B <- B + ygk*A
    A <- A%*%hatP
    suma <- suma + ygk
  }
  return(round(B/rlimit, 4))
}
```

::: example
Retomamdo el sistema sobre el tiempo de vida de una máquina descrito en el ejemplo \@ref(exm:excmtc005), supongamos que el tiempo esperado hasta que falla una máquina son 10 días, mientras que el tiempo esperado de reparación es de 1 día. Si la máquina funciona el primer día de enero ¿cuál es el tiempo total esperado de funcionamiento de la máquina al finalizar el mes de enero? Dado que el proceso sólo tiene espacio de estados $S = \{0, 1\}$, la cantidad de interés es $m_{11}(31)$. Con $\lambda = 1$ y $\mu = 0.1$, la matriz de tasas del proceso viene dada por:
:::


```r
nestados <- 2
lambda <- 1
mu <- 0.1
R <- matrix(nrow = nestados, ncol = nestados, data = 0)

R[1,2] <- lambda 
R[2,1] <- mu 
```

Obtenemos ahora la matriz $M(31)$ correspondiente a la cantidad de interés, teniendo en cuenta que partimos de que la máquina está en marcha (primera fila de $M$) al inicio:


```r
tiempos.ocupacion(R, 31, 1)
```

```
##        [,1]    [,2]
## [1,] 3.6446 27.3554
## [2,] 2.7355 28.2645
```

Por tanto, el tiempo esperado de funcionamiento es de 28.26 días, mientras que el tiempo de reparación es de $2.74$ días. Utilizando el simulador del proceso podemos ver que el resultado obtenido es similar.


```r
# Réplicas del proceso
replicas <- 2500
envs <- lapply(1:replicas, function(i) {
    maquina <- sistema.1m(31, 1, 1/10)
})
# almacenamos análisis de llegadas del sistema
simarrivals <- as_tibble(get_mon_arrivals(envs))
# Almacenamos el estado final de la cola en el último instante del sistema
simarrivals %>%
  mutate(tOFF = end_time - start_time) %>%
  group_by(replication) %>%
  summarise(totalOFF = sum(tOFF)) %>%
  ungroup() %>%
  summarise(mOFF = mean(totalOFF), mON = 31 - mOFF)
```

```
## # A tibble: 1 x 2
##    mOFF   mON
##   <dbl> <dbl>
## 1  3.33  27.7
```

*Para practicar este apartado puedes resolver los ejercicios B-5 a B-8 de la colección al final de la unidad.*

## Comportamiento límite del proceso {#CMTCH}

En el análisis del comportamiento límite de una CMTD analizamos la distribución de probabilidad límite, la distribución estacionaria, y la distribución de los tiempos de ocupación. En el caso de las CMTC estudiaremos cantidades similares. En primer lugar analizaremos las probabilidades límite:

$$\lim_{t \rightarrow \infty} P(X(t) = j, \quad 1 \leq j \leq N$$

Si existen dichos límites el conjunto $p = [p_1, p_2,...,p_N]$ se conoce como distribución límite de la CMTC.

::: theorem
Una CMTC $\{X(t), t \geq 0\}$ irreducible con matriz de tasas $R$ tiene una única distribución límite $p = [p_1, p_2,...,p_N]$, que se puede obtener como solución de las ecuaciones de balance:

$$p_j r_j = \sum_{i=1}^N p_ir_{ij}, \quad 1 \leq j \leq N$$ $$\sum_{i=1}^N p_i = 1$$
:::

En este sentido podemos interpretar $p_jr_j$ como la tasa de la CMTC cuanod deja el estado $j$, mientras que $p_jr_{ij}$ es la tasa de entrada de la CMTC a estado $j$ desde el estado $i$.

::: theorem
Dado una CMTC $\{X(t), t \geq 0\}$ irreducible con distribución límite $p$, entonces la distribución estacionaria de la CMTC viene dada por $p$, es decir:

$$P(X(0) = j) = p_j \text{ para } 1 \leq j leq N$$ $P(X(t) = j) = p_j \text{ para } 1 \leq j leq N, t \geq 0$\$
:::

A partir de la distribución límite resulta posible obtener la distribución de ocupación de la CMTC.

::: theorem
Sea $m_{ij}(T)$ el tiempo total esperado que la cadena permanece en el estado hata el tiempo $T$ para una CMTC irreducible que comienza en el estado $i$. Entonces:

$$\lim_{T \rightarrow \infty} \frac{m_{ij}(T)}{T} = p_j$$
:::

A continuación se presenta la solución de la distribución límite para los procesos de nacimiento y muerte. En el resto de sistemas se deberan plantear las ecuaciones de balance y resorverlas. En ambas situaciones presentamos las correspondientes funciones que nos permiten obtener las cantidades de interés.

::: {.bluebox data-latex=""}

Sea $\{X(t), t \geq 0\}$ un proceso de nacimiento y muerte con espacio de estados $S = \{0, 1,...,K\}$, y tasas de nacimiento $\{\lambda_i, 0 \leq i < K\}$ y tasas de muerte $\{\mu_i, 1 \leq i \leq K\}$. Entonces la CMTC así definida es irreducible y tiene una única distribuión límite con:

$$p_i = \frac{\rho_i}{\sum_{j = 0}^K \rho_j}, \quad 0 \leq i \leq K,$$ donde $\rho_0 = 1,$ y

$$\rho_i = \frac{\prod_{j=0}^{i-1}\lambda_j}{\prod_{j=1}^{i}\mu_j}, \quad 1 \leq i \leq K,$$
:::

Antes de comenzar con los ejemplos vamos a crear una función que permita obtener la distibución límite y la distribución de ocupación para los procesos de nacimiento y muerte.


```r
# Obtención de distribuciones límite 
distr.lim.nm <- function(estados, lambdas, mus)
{
  # Parámetros de la función
  # ========================
  # estados: número de estados del sistema
  # lambdas: vector de tasas de nacimiento
  # mus: vector de tasas de muerte
  
  # definimos vector de rho
  rhos <- rep(1, estados)
  # calculamos productos acumlados para lambda y mu
  prl <- cumprod(lambdas)
  prm <- cumprod(mus)
  # rellenamos rho con los productos acumulados
  rhos[2:estados] <- prl/prm
  # suma de rhos
  sumarhos <- sum(rhos)
  # vector de probabilidades
  ps <- rhos/sumarhos
  return(ps)
}
```

::: example
Retomamdo el sistema descrito en el ejemplo \@ref(exm:excmtc005), supongamos que el tiempo esperado hasta que falla una máquina son 10 días, mientras que el tiempo esperado de reparación es de 1 día. Si la máquina funciona el primer día de enero ¿cuál es la distribución límite del proceso?

Este sistema es un proceso de nacimiento y muerte donde podemos aplicar la función anterior para obtener la distribución límite con dos estados y tasas $\lambda = 1$, $\mu = 1/10$ (expresadas en periodos de diez días).
:::


```r
probs <- distr.lim.nm(2, 1, 0.1)
probs
```

```
## [1] 0.09090909 0.90909091
```

El comportamiento límite nos indica que la máquina está el 90.9% del tiempo en funcionamiento, mientras que sólo el 9.1% en reparación. Para un periodo de un año tendríamos que los díass esperados de reparación y funcionamiento son:


```r
365*probs
```

```
## [1]  33.18182 331.81818
```

Veamos ahora como obtener la distribución límite para procesos más generales. Definimos una función que nos permite obtener la distribución límite de un CMTC a partir de cualquier matriz de tasas resolviendo las ecuaciones de balance.


```r
# Función para la resolución numérica de las ecuaciones de balance
distr.lim.general<-function(Rmat)
{
  # Parámetros de la función
  #=========================
  # Rmat: matriz de tasas del sistema
  
  # número de estados del sistema
  estados <- nrow(Rmat)
  # Calculamos r_i y lo colocamos en formato matriz
  sumarows <- diag(apply(Rmat, 1, sum), estados)
  # Matriz de coeficientes del sistema de ecauciones de balance
  A <- t(R)-sumarows
  # Completamos la matriz añadiendo la restricción de suma de p`s iagual a 1
  A <- rbind(A, rep(1, estados))
  # Vector de términos independientes del sistema
  CS <- c(rep(0, estados), 1)
  # Resolcuión del sistema
  ps <- qr.solve(A, CS)
  return(ps)
}
```

::: example
Para el sistema de proceso de fabricación se está interesado en conocer cuando la máquina estará parada a largo plazo. Dado que el espacio de estados es $S = \{1, 2,...,6\}.$ la máquina está aprada cuando nos encontramos en los estados $5 = (4, 0)$ y $6 = (3, 0)$. A partir de la información del sistema podemos obtener la distribución límite del proceso, pero en este caso como no se trata de un proceso de nacimiento y muerte debemos plantear las ecuaciones de balance, a partir de la matriz de tasas, y resolver el sistema numéricamente.
:::

Resolvemos las ecuaciones de balance para el sistema del proceso de fabricación. Definimos la matriz de tasas y ejecutamos la función anterior para obtener las probabilidades límite del proceso:


```r
# Estados del sistema
nestados <- 6
# Matriz de tasas
lambda <- 6
mu <- 5
R <- matrix(nrow = nestados, ncol = nestados, data = 0)
R[1,2] <- lambda 
R[2,1] <- mu
R[2,3] <- lambda 
R[3,2] <- mu
R[3,4] <- lambda
R[4,3] <- mu
R[4,5] <- lambda
R[5,6] <- mu
R[6,3] <- mu
# Resolución  de las ecuaciones de balance
ps <- distr.lim.general(R)
ps
```

```
## [1] 0.1584649 0.1901579 0.2281895 0.1244670 0.1493604 0.1493604
```

La probabilidad de interés viene dada por 0.2987, de forma que la máquina permancerá apagada sobre el 30% del tiempo.

*Para practicar este apartado puedes resolver los ejercicios B-9 a B-12 de la colección al final de la unidad.*

## Análisis de costes {#CMTCI}

En este punto vemos como podemos introducir costes en las CMTC y los procedimientos numéricos necesarios para su análisis. En todo el punto consideramos $\{X(t), t \geq 0\}$ una CMTC con espacio de estados $S = \{1, 2,...,N\}$ y matriz de tasas $R$. Además, siempre que la CMTC está en el estado $i$ se incurre en una tasa de coste $c(i), 1 \leq i \leq N$.

### Coste total esperado a tiempo $T$

En esta subsección, estudiamos el **CTE**, el *coste total esperado* hasta un tiempo finito $T$, llamado horizonte. Nótese que la tasa de coste en el tiempo $t$ es $c(X(t))$. Por lo tanto, el coste total hasta el tiempo $T$ viene dado por:

$$\int_0^T c(X(t))dt.$$

De esta forma el coste esprado total hasta el instante $T$, empezando en el estado $i$, viene dado por:

$$g(i, T) = E\left( \int_0^T c(X(t))dt \mid X(0) = i \right), \quad 1 \leq i \leq N.$$ 

::: {.theorem}

Si $M(T) = [m_{ij}(T)]$ es la matriz de ocupación entonces:

$$g(T) = M(T)c,$$

donde $c = [c(1), c(2),...,c(N-1), c(N)]'$ y $g(T) = [g(1, T), g(2,T),..., g(N-1, T), g(N, T)]'.$ 
:::

::: example
Para el sistema de mantenimiento de máquinas se conoce que que el beneficio por cada hora que la máquina está funcionanado es de 50 euros, mientras que el coste de que la máquina este apagada es de 15 euros por hora, al que hay que sumar 10 euros por cada hora de reparación. Estamos interesados en conocer el coste-beneficio de un periodo de 24 horas si al finalizar todas las máquinas están funcionando. Si $X(t)$ es el número de máquinas funcionando en el instante $t$, el espacio de estados para 4 máquinas viene dado por $S = \{0, 1, 2, 3, 4\}$ y el vector de costes es:
:::

$$
\begin{matrix}
c(0) = & 0*50 - 4*15 - 2*10 = - 80,\\
c(1) = & 1*50 - 3*15 - 2*10 = - 15,\\
c(2) = & 2*50 - 2*15 - 2*10 = 50,\\
c(3) = & 3*50 - 1*15 - 1*10 = 125,\\
c(4) = & 4*50 - 0*15 - 0*10 = 200.
\end{matrix}
$$ El vector de costes para el periodo de 24 horas viene dado por:

$$
g(24) = M(24)*c =
\begin{bmatrix}
3844.69 \\
4116.57 \\
4327.23 \\
4474.96 \\
4621.05
\end{bmatrix}
$$

de forma que la cantidad de interés, $g(4, 24)$ que corresponde con el elemento $(5,1)$ de la matriz, establece un beneficio de 4621.05 euros. Veamos como obtener esta cantidad utilizando el código correspondiente.


```r
# Matriz de tasas
nestados <- 5
R <- matrix(nrow = nestados, ncol = nestados, data = 0)
lambda <- 1/2
mu <- 1/72 

R[1,2] <- 2*lambda 
R[2,1] <- mu 
R[2,3] <- 2*lambda 
R[3,2] <- 2*mu 
R[3,4] <- 2*lambda 
R[4,3] <- 3*mu 
R[4,5] <- lambda
R[5,4] <- 4*mu
# Matriz de ocupación
mmat <- tiempos.ocupacion(R, 24, 1)
# Vector de costes
costes <- matrix(c(-80, -15, 50, 125, 200), ncol = 1)
# Matriz de beneficios
beneficios <- mmat%*%costes
beneficios
```

```
##          [,1]
## [1,] 3844.694
## [2,] 4116.585
## [3,] 4327.238
## [4,] 4474.971
## [5,] 4621.058
```

El beneficio es 4621.058 euros.

### Tasas de coste a largo plazo

Para el sistema de `vida de una máquina`, supongamos que se da el coste $C$ del tiempo de inactividad. Queremos saber a cuánto debe ser la tasa de ingresos durante el tiempo de actividad para que sea económicamente rentable operar la máquina. Si nos guiamos por el coste total, la respuesta dependerá de del horizonte de planificación $T$ y también del estado inicial de la máquina. Una alternativa es calcular los ingresos netos a largo plazo por unidad de tiempo para esta máquina e insistir en que sea positivo para la rentabilidad. Esta respuesta no dependerá de $T$, y, como veremos ni siquiera del estado inicial de la máquina. Por lo tanto, el cálculo de estos índices de costes o ingresos a largo plazo es muy útil. En esta subsección mostraremos cómo calcular estas cantidades.

::: theorem
Sea $\{X(t), t \ geq 0\}$ una CMTC irreducible con estados $\{1, 2,...,N\}$, distribución límite $p = [p_1, p_2,...,p_N]$ y vector de costes $c = [c_1, c_2,..., c_N]$, entonces la tasa de coste a largo plazo viene dada por:

$$g(i) = \sum_{j = 1}^N p_jc(j), \quad 1 \leq i \leq N$$
:::

::: example
Si consideramos el sistema de `vida de una máquina` supongamos que el coste por unidad de tiempo de que la máquina este apagado es $C$. ¿Cuál es el la tasa mínima de ingresos $I$ necesaria durante el tiempo de actividad para alcanzar el punto de equilibrio a largo plazo?
:::

Utilizando las tasas definidas anteriormente ($\lambda = 1, \mu = 0.1$), la distribución límite del sistema $p = [0.0909, 0.9091]$, y el vector de costes $c =[-C, I]$ para el espacio de estados $S = \{0, 1\}$, la tasa de coste a largo plazo por unidad de tiempo viene dada por la expresión:

$$ g = 0.9091*I - 0.0909*C$$ de forma que para mantener el sistema en equilibrio, $g \geq 0$, se debe cumplir que:

$$I \geq \frac{0.0909}{0.9091}*C = 0.099989*C \approx 0.1*C$$

Así, los ingresos por unidad de tiempo superiores a 0.1 veces por el coste de que la máquina este parada por unidad de tiempo resulta rentable. Si la empresa ha establecido un ingreso por hora de 50 euros, cuando sabe que el coste por hora es de 10 euros cuando está apagada, se desea saber si se cumple la condición de equilibrio para los costes.

::: example
Consideramos el sistema de la `central telefónica` donde la capacidad máxima de la centralita es de seis llamadas. Las llamadas llegan según un $PP$ con una tasa de 4/minuto, y la duración media de cada llamada exponencial de media 2 minutos. En primer lugar deseamos conocer le beneficio por unidad de tiempo si la facturación por minuto de cada llamada es de 10 céntimos. En segundo lugar deseamos estimar la pérdida que sufrimos por todas las llamadas que no pueden ser atendidas. Consideramos como $X(t)$ al número de llamadas que están siendo atendidas en el instante $t$.
:::

En primer lugar calculamos la distribución límite del proceso. Dado que se trata de un proceso de nacimeinto y muerte utilizamos la función correspondiente.


```r
# Matriz de tasas
nestados <- 7
lambdas <- rep(4, 6) 
mus <- (1:6)/2
# Probabilidades del sistema
probs <- distr.lim.nm(nestados, lambdas, mus)
```

Establecemos los beneficios $c(i) = 10i$ y calculamos el global por unidad de tiempo


```r
# vector de beneficios
beneficio <- 10*(0:6) 
# beneficio por unidad de tiempo
sum(beneficio*probs)
```

```
## [1] 48.81985
```

El beneficio por unidad de tiempo es de 48.82 céntimos. Si no rechazaramos ninguna llamada tendríasmo que el beneficio por minuto sería de 80 céntimos, que se corresponde con la tasa de 4 llamadas por minuto, el beneficio de 10 céntimos por minuto, y que a duración de las llamadas es de 2 minutos. Esto supone que la pérdida por minuto debido a las lamadas rechazadas se puede estimar como $80 - 48.82 = 31.18$ céntimos por minuto.

## Tiempos de primer paso {#CMTCJ}

Como ocurría con las CMTD podemos hablar de los tiempos de primer paso en las CMTC. Si $\{X(t), t \geq 0\}$ es una CMTC con espacio de estados $\{1, 2,...,N\}$ y matriz de tasas $R$, entonces se define como el **tiempo de primer paso al estado** $N$ como:

$$T = min\{t \geq 0: X(t) = N\}.$$

Más concretamente estudíamos el valor esperado de $T$, $E(T)$. Para ello definimos los tiempos esperados a partir de cada uno de los estados del sistema como:

$$m_i = E(T \mid X(0) = i), \quad 1 \leq i \leq N-1$$ y $m_N = 0.$

::: theorem
Los valores $\{m_i, 1 \leq i \leq N-1\}$ satisfacen

$$r_im_i = 1 + \sum_{j=1}^{N-1} r_{ij}m_j, \quad 1\leq i \leq N-1.$$
:::

Si deseamos calcular los tiempos de primer paso a un subconjunto de estados, $A$ se puede adaptar el teorema anterior ya que sólo debemos resolver el sistema:

$$r_im_i(A) = 1 + \sum_{j \notin A} r_{ij}m_j(A), \quad 1\leq i \leq N-1.$$

A continuación presentamos un algoritmo para calcular los valores de $m_i$ a partir de la matriz de tasas del sistema, donde debemos indicar el estado o estados desde los que partimos al inicio.


```r
# Función para la resolución numérica de las ecuaciones de balance
tiempos.primer.paso<-function(Rmat, A, estados)
{
  # Parámetros de la función
  #=========================
  # Rmat: matriz de tasas del sistema
  # A: vector de estados que debemos alcanzar
  # estados: conjunto de estados total (como carácter)
  
  # Estados como texto
  
  estados <- as.character(estados)
  # Tasas r
  rts <- diag(apply(Rmat, 1, sum), nrow(Rmat))
  # Seleccionamos el subconjunto de la matriz quitando fila y columna
  Rmod <- Rmat[-A, -A]
  rates <- rts[-A, -A]
  # Número de m´s a estimar
  lms <- nrow(Rmod)
  # Matriz de coeficientes del sistema de ecuaciones de balance
  B <- rates - Rmod
  # Vector de términos independientes del sistema
  CS <- rep(1, lms)
  # Resolución del sistema
  ps <- as.data.frame(qr.solve(B, CS))
  rownames(ps) <- paste("estado", estados)[-A]
  colnames(ps) <-"tiempo"
  return(ps)
}
```

::: example
En las condiciones del sistema $M/M/1/K$ del `cajero bancario` descrito anteriormente, estamos interesados en conocer el tiempo quen debe trasncurrir hasta que la cola esta vacia si ahora mismo hay un cliente en el sistema y cero en la cola ($X(0) = 0$). Recordemos que el esapcio de estados hace referencia al número de clientes en la cola y viene dado por $\{0, 1, 2, 3, 4, 5\}.$
:::


```r
# Definición matriz de tasas
nestados <- 6
R <- matrix(nrow = nestados, ncol = nestados, data = 0)
R[1,2] <- 10
R[2,1] <- 15 
R[2,3] <- 10 
R[3,2] <- 15 
R[3,4] <- 10 
R[4,3] <- 15 
R[4,5] <- 10
R[5,4] <- 15
R[5,6] <- 10
R[6,5] <- 15

# Tiempos de primer el estado 1 que deseamos alcanzar (corresponde con el primer elemento del espacio de estados)
tiempos.primer.paso(R, 1, 0:5)
```

```
##             tiempo
## estado 1 0.1736626
## estado 2 0.3341564
## estado 3 0.4748971
## estado 4 0.5860082
## estado 5 0.6526749
```

El tiempo esperado hasta que la cola este vacia de nuevo son 0.1736 horas o 10.42 minutos.

::: example
En las condiciones del sistema de `mantenimiento de areonaves` descrito anteriormente. Supongamos que en un experimento de prueba el avión despega con cuatro motores motores que funcionan correctamente y sigue volando hasta que se estrella. Estamos interesados en conocer el tiempo esperado del accidente.

Recordemos que el espacio de estados del sistema es $\{1, 2,...,,9\}$, y sabemos que el avión se estrallará si en algún momento accedemos al subconjunto de estados $\{1, 2, 3, 4, 7\}.$ Calculamos los tiempos de primer paso cuando $X(0) = \{5, 6, 8, 9\}$, aunque como el avión está en condiciones çoptimas para despegar nos debermeos fijar en el valor correspondiente al estado 9. Recordemos que el tiempo medio hasta que falla un motor es de 200 horas, de forma que $\lambda = 1/200 = 0.005.$
:::


```r
# Definición matriz de tasas
nestados <- 9
lambda <- 0.005
R <- matrix(nrow = nestados, ncol = nestados, data = 0)
R[2,1] <- lambda 
R[3,2] <- 2*lambda 
R[4,1] <- lambda 
R[5,2] <- lambda 
R[5,4] <- lambda 
R[6,3] <- lambda 
R[6,5] <- 2*lambda
R[7,4] <- 2*lambda
R[8,5] <- 2*lambda
R[8,7] <- lambda
R[9,6] <- 2*lambda
R[9,8] <- 2*lambda

# Conjunto que debemos alcanzar
A <- c(1, 2, 3, 4, 7)
# Tiempos de primer paso 
tiempos.primer.paso(R, A, 1:9)
```

```
##            tiempo
## estado 5 100.0000
## estado 6 133.3333
## estado 8 133.3333
## estado 9 183.3333
```

De esta forma, partiendo de un avión en condiciones óptimas el tiempo ahsta que ocurra un incidente que le impida volar es de 183.33 horas, o lo que es lo mismo 183 horas y 20 minutos.

## Ejercicios {#CMTCK}

Los ejercicios que se presentan a continuación se estruturan en dos niveles de dificultad. El primer nivel son ejercicios más básicos (codificados con una B), y que son parte de los ejmplos rabajados en la unidad, mientras que el segundo bloque necesitan una mayor cantidad de trabajo (codificados con una A). Al final de los ejercicios se encuentra el código de `R` para resolver algunos de dichos ejercicios. Cuando consideres necesario puedes plantear una solución mediante simulación para contestar a las preguntas de interés.

**Ejercicio B-1.** Para el proceso de `mantenimiento de máquinas` estamos interesados en calcular el valor esperado del número de máquinas en funcionamiento después de 9 horas de funcionamiento, suponiendo que el sistema empieza con todas las máquinas paradas.

**Ejercicio B-2.** Para el proceso de `mantenimiento de aeronaves` supongamos que cada motor funciona en promedio unas 200h antes de detectarse cualquier problema. Si los cuatro motores están funcionando antes de comenzar un vuelo de seis horas ¿cuál es la probabilidad de que el vuelo llegue de forma segura? (Hint: Para seolver este ejercico ten en cuenta la codificación de estados establecida en la definición del sistema).

**Ejercicio B-3.** Para el proceso de `centralita telefónica` supongamos que la capacidad total de la centralita es de 10 llamadas y que se reciben llamadas a una tasa de 1 por minuto, y que el tiempo medio de atención de cada llamada es de 10 minutos. Si ahora mismo la centralita está atendiendo a 3 llamadas:

-   ¿cuál es la probabilidad de que la centralita este como máximo al 50% de su capacidad dentro de media hora? ¿y dentro de una hora?.
-   Si la centralita está activa durante 6 horas más ¿cuantas llamadas estarán pendientes al finalizar el tiempo de trabajo?

**Ejercicio B-4.** Para el proceso de `sistema de inventarios` supongamos que la capacidad máxima de stock del producto $KD$ es 20, y que se solicitan nuevas piezas cuando el stock es inferior a 6. Además tenemos que la tasa de entrega de nuevos productos es de tres días, mientras que la demanda de dicho producto es de 3 piezas/dia. Si en estos momentos no hay stock de la pieza $KD$ la empresa esta interesada en conocer ¿cuál es el stock más probable dentro de 8 días? ¿cuál es la probabilidad de que tengamos que reabastecernos al finalizar de los ocho días?

**Ejercicio B-5.** Para el proceso $M/M/1/K$ del `cajero bancario` supongamos que los clientes llegan con una tasa de 10 por hora y que tardan en promedio unos cuatro minutos en realizar las operaciones con el cajero. Supongamos que hay espacio para como máximo cinco clientes delante del cajero automático, mientras que un cliente está siendo atendido. Si el cajero está inactivo ¿cuál es el tiempo esperado de inactividad del cajero durante la siguiente hora?.

**Ejercicio B-6.** Para el `proceso de fabricación` consideramos la situación siguiente. Supongamos que el sistema funciona las 24 horas del día, las demandas se producen a cinco por hora y el tiempo medio de fabricación de un artículo es de 10 minutos. La máquina se pone en marcha cuando el stock de artículos fabricados se reduce a dos, y permanece encendida hasta que las existencias aumentan a cuatro, momento en el que se apaga. Supongamos que el stock es de cuatro (y la máquina está apagada) al principio. Estamos interesados en la cantidad de tiempo esperada durante el cual la máquina está encendida durante las siguientes 24 horas.

**Ejercicio B-7.** Para el proceso de `sistema de inventarios` supongamos que la capacidad máxima de stock del producto $KD$ es 20, y que se solicitan nuevas piezas cuando el stock es inferior a 6. Además tenemos que la tasa de entrega de nuevos productos es de tres días, mientras que la demanda de dicho producto es de 3 piezas/dia. Si en estos momentos no hay stock de la pieza $KD$ la empresa esta interesada en conocer ¿cuál es el tiempo esperado durante el cúal se debe reabastecer el almacén durante los próximos 8 días?

**Ejercicio B-8.** Para el proceso de `centralita telefónica` supongamos que la capacidad total de la centralita es de 10 llamadas y que se reciben llamadas a una tasa de 1 por minuto, y que el tiempo medio de atención de cada llamada es de 10 minutos. Si ahora mismo la centralita está atendiendo a 3 llamadas ¿cuál es el tiempo esperado de que el sistema este por encima del 80% de ocupación? ¿y por debajo del 20%? ¿cuál es el tiempo esperado de que el sitema este a plena capacidad?

**Ejercicio B-9.** Para el sistema de `mantenimiento de máquinas` consideramos cuatro máquinas disponibles y dos operarios para repararlas en caso de fallo con tiempos de vida de las máquinas exponenciales con media de 3 días, y tiempos medios de reparación de 2 horas. Estamos interesados en la probabilidad a largo plazo de que todas las máquinas estén en funcionamiento, y en la fracción de tiempo a largo plazo que los dos operarios están ocupados.

**Ejercicio B-10.** Para el proceso $M/M/1/K$ del `cajero bancario` supongamos que los clientes llegan con una tasa de 10 por hora y que tardan en promedio unos cuatro minutos en realizar las operaciones con el cajero. Supongamos que hay espacio para como máximo cinco clientes delante del cajero automático, mientras que un cliente está siendo atendido. ¿Cómo interpretamos estas probabilidades? ¿Cuál es la probabilidad de que tengamos más de dos clientes en la cola? ¿y de que tengamos como máximo 2?

**Ejercicio B-11.** Para el sistema de `mantenimiento de aeronaves` estamos interesados en conocer cual es la probabilidad límite de que el avión no pueda finalizar el vuelo.

**Ejercicio B-12.** Para el sistema del `vendedor por ciudades` viajante estamos interesados en conocer a la largo plazo cual es la proporción de tiempo que permanecerá en cada ciudad. En este caso la matriz de tasas debe tener en cuenta las probabilidades de moverse de una ciudad a otra. Si el beneficio que obtiene el vendedor es de 80 euros/día en la ciudad A, 100 euros/día en la ciudad B, 125 euros/día en C, y que además se incurre en un gasto por desplazamiento de 25 céntimos por kilómetro caundo hay 50 kilómetros entre A y B, 65 kilómetros entre A y C, y 80 kilómetros entre B y C ¿cuál es el beneficio total esperado del sistema para un periodo de un mes? ¿Cuál es la tasa de coste a largo plazo? Si el viajante comienza su viaje en la ciudad A ¿cuál es el tiempo esperado hasta que el viajante vuelva a la ciudad A?

**Ejercicio B-12.** Un taller mecánico consta de dos taladradoras y dos tornos. Los tiempos de vida de las taladradoras son variables aleatorias $Exp(\mu_b)$ y las de los tornos son variables aleatorias $Exp(\mu_l)$. El taller mecánico tiene dos reparadores: Al y Bob. Al puede reparar tanto tornos como taladros, mientras que Bob sólo puede reparar tornos. Los tiempos de reparación de las taladradoras son $Exp(\lambda_b$ y para los tornos $Exp(\lambda_l)$, independientemente de quién repare las máquinas. Las taladradoras tienen prioridad en las reparaciones. Las reparaciones pueden adelantarse. Haciendo las suposiciones de independencia apropiadas independencia, modelar este taller mecánico como un CMTC, considerando el proceso $A(t) = (b, l)$ que indica el número de taladros y tornos en funcionamiento en el instante $t$ y obtener la correspondiente matriz de tasas.

**Ejercicio A-1** Un fondo de inversión se clasifica en cuatro estados según los beneficios que produce por unidad de tiempo: altos, medios, bajos, y pérdidas. Además, el movimiento entre estados puede ser visto como una CMTC con matriz de tasas:

$$
R = \begin{pmatrix}
0 & 0.1 & 0.1 & 0\\
0 & 0 & 0.3 & 0.1\\
0 & 0 & 0.5 & 0.5\\
1.5 & 0 & 0  & 0 
\end{pmatrix}
$$ Mientras que el sistema está en cada uno de los estados, el beneficio respectivo estimado es de 500, 250, 100, y -600 euros por unidad de tiempo.

-   ¿cuál es el coste total esperado para un periodo de 10 unidades de tiempo?
-   ¿cuál será la tasa de coste a largo plazo?
-   Si ahora mismo estamos en pérdidas ¿cuánto tiempo tiene que pasar hasta que alcanzemos beneficios altos?

La empresa considera que si el coste del estado de pérdidas se duplicará (pasar de 600 a 1200) mejorarían los datos de las cuestiones anteriores ¿qué lo podemos decir a la empresa?

**Ejercicio A-2.** Sea $\{X(t), t \geq 0\}$ una CMTC con estados $\{1, 2, 3, 4, 5\}$ y matriz de tasas

$$
R = \begin{pmatrix}
0 & 4 & 4 & 0 & 0\\
5 & 0 & 5 & 5 & 0\\
5 & 5 & 0 & 4 & 4\\
0 & 5 & 5  & 0 & 4\\
0 & 0& 5& 5& 0
\end{pmatrix}
$$

-   Calcular la matriz de tiempos de ocupación para $t=0.2$.
-   Obtener la distribución límite del proceso.
-   Si el sistema incurre en costes de acuerdo a la ecuación $c(i) = 2i+1, \quad 1\leq i\leq 5$ ¿Cuál el coste total esperado en el intervalo de tiempo $[0, 10]$, si el sistema está ahora mismo en el estado 2. ¿Cuál sería la tasa de coste a largo plazo?
-   ¿Cuál es el tiempo esperado para ir del estado 1 al estado 5?

**Ejercicio A-3.** Sea $\{X(t), t \geq 0\}$ una CMTC con estados $\{1, 2, 3, 4, 5, 6\}$ y matriz de tasas

$$
R = \begin{pmatrix}
0 & 6 & 6 & 8 & 0 & 0\\
0 & 0 & 6 & 8 & 0 & 0\\
0 & 0 & 0 & 6 & 4 & 0\\
0 & 6 & 8  & 0 & 0 & 0\\
6 & 0& 0& 0& 6 & 0
\end{pmatrix}
$$

-   Calcular la matriz de tiempos de ocupación para $t=0.1$.
-   Obtener la distribución límite del proceso.
-   Si el sistema incurre en costes de acuerdo a la ecuación $c(i) = 2i^2+3, \quad 1\leq i\leq 6$ ¿Cuál el coste total esperado en el intervalo de tiempo $[0, 15]$, si el sistema está ahora mismo en el estado 4. ¿Cuál sería la tasa de coste a largo plazo?
-   ¿Cuál es el tiempo esperado para ir del estado 6 al estado 4?

**Ejercicio A-4.** Un peso de $18$ toneladas está sostenido por $3$ cables que se reparten la carga por igual. Cuando uno de los cables se rompe, los restantes, que no se han roto, se reparten la carga a partes iguales. Cuando se rompe el último cable, se produce un fallo. La tasa de fallos de un cable es $0.2$ por año y tonelada. Los tiempos de vida de los $3$ cables son independientes entre sí. Se considera $X(t)$ como el número de cables que siguen sin romperse en el momento $t$.

-   ¿Cuál es la probabilidad de que el sistema dure más de dos años?
-   ¿Cuál es el tiempo estimado hasta que el sistema falle si ahora los tres cables están bien?

**Ejercicio A-5.** Un ordenador tiene cinco unidades de procesamiento (CPUs). Los tiempos de vida de las CUPs son variables aleatorias iid exponenciales de media 2 años. Cuando una CPU falla, el ordenador intenta aislarla automáticamente y reconfigurar el sistema con las demás CPU. Sin embargo, este proceso tiene éxito con una probabilidad $0.94$, denominada factor de cobertura. Si la reconfiguración tiene éxito el sistema continúa con una CPU menos. Si el proceso falla, todo el sistema se bloquea. Supongamos que el proceso de reconfiguración es instantáneo y que una vez que el sistema se bloquea, se detiene definitivamente. Se considera $X(t)$ igual a cero si el sistema ha dejado de funcionar en el instante $t$, o en caso contrario es igual al número de CPUs en funcionamiento en el momento $t$.

-   ¿Cuál es la probabilidad de que los cinco procesadores funcionen 5 años sin fallos? (asumimos que todos los procesadores están en funcionamiento en el instante 0)

Imaginemos ahora que el sistema puede ser reparado cuando falla. El tiempo enecesario para la reparación es una variable aleatoria exponencial de media 5 días, y una vez se finaliza todas las CPUS funcionan de nuevo.

-   ¿Cuál es la probabilidad límite de que el sistema este en reparación?.
-   ¿cuál es el tiempo esperado hasta que el sistema falla y debe ser reparado si en estos momentos funcionan todos los procesadores?.
-   Supongamos que cada hora de trabajo de cada procesasor proporciona 100 euros de beneficio, mientras que las reparaciones cuestan 200 euros/hora. ¿cuál es el beneficio esperado durante el primer año si los cinco procesadores están trabajndo ahora mismo?

**Ejercicio A-6.** Una estación de servicio tiene tres servidores (1, 2, y 3). Cuando llega un cliente, es asignado al servidor libre con el índice más bajo. Si los servidores están ocupados el cliente no se detiene y deja la estación de servicio. Los tiempos de servicio de cada servidor son variables aletorias $Exp(\mu_i)$ con:

-   media de 8 minutos para el servidor 1,\
-   la media del servidor 2 es dos veces más rápido que el servidor 3,
-   la media del servidor 1 es dos veces más rápido que el servidor 2.

Los clientes llegan de acuerdo a un $PP(\lambda)$ con tasa de 20 clientes por hora.

-   ¿cuál es la probabilidad de que los tres servidores esten ocupados a los 20 minutos, asumiendo que el sistema está vacio en el instante 0?
-   ¿cuál es el tiempo de ocupación de cada estado del sistema en 50 minutos, asumiendo que el sistema está vacio en el instante 0?
-   ¿cuál es el tiempo esperado hasta que los tres servidores estén libres, asumiendo qeu el sistema tiene un cliente?
-   Si los costes de los tres servidores son 40, 20 y 10 euros por hora respectivamente ¿cuál el valor de $c$ más pequeño que hace que el sistema sea rentable a largo plazo?

**Ejercicio A-7.** Una estación de servicio monoservicio atiende a dos tipos de clientes. Los clientes de tipo $i, i = 1, 2,$ llegan según un $PP(\lambda_i)$ independientes. La estación tiene espacio para atender como máximo a $K$ clientes. Los tiempos de servicio son variables aleatorias iid $Exp(\mu)$ para ambos tipos de clientes. La política de admisión es la siguiente. Si, en el momento de una llegada, el número total de clientes en el sistema es $M$ o menos (aquí $M < K$ es es un número entero fijo), se permite que el cliente que llega se incorpore a la cola; en caso contrario sólo si es del tipo 1. Esto crea un trato preferente para los clientes de tipo 1. Sea $X(t)$ el número de clientes (de ambos tipos) en el sistema en el tiempo $t$ . Si las tasas de llegadas son de 5 y e minutos, la tasa de servicio es de 4 minutos, $K = 5$, y $M = 3$.

-   ¿Cuál es la variable de interés del sistema?
-   ¿Cuáles son los tiempos de ocupación en el periodo de 60 minutos desde el inicio del servico, si en el instante inicial no hay clientes?
-   ¿Cuál es la probabilidad a largo plazo de que la estación esté vacía?
-   Si al abrir la gasolinera tenemos un cliente ¿cuánto tiempo debe pasar hasta que rechazemos el primer cliente?

**Ejercicio A-8** Una pequeña gasolinera tiene un surtidor y espacio para un total de tres coches (uno en el en el surtidor y dos en espera). El tiempo entre las llegadas de los coches a la estación es una variable aleatoria exponencial con una tasa media de llegada de 10 coches por hora. El tiempo que cada coche pasa delante del surtidor es una variable aleatoria exponencial con una media de cinco minutos (es decir, una tasa media de 12 por hora). Si hay tres coches en la estación y llega otro coche, el coche recién llegado sigue su camino y nunca entra en la estación. Considera $X(t)$ como el número de coches en la estación en el momento $t$.

-   ¿Cuál es la probabilidad a largo plazo de que la estación esté vacía?
-   ¿Cuál es el número esperado de coches en la estación a largo plazo?
-   ¿Cuál es la proporción de tiempo que la estación estará completa en un periodo de 8 horas?
-   Si al abrir la gasolinera tenemos un cliente ¿cuánto tiempo debe pasar hasta que no tengamos ningún cliente en el sistema? ¿y más de uno?

**Ejercicio A-9 (Cola** $M^x/M/1/K$). Una pequeña tienda de autoservicio 24 horas de carretera tiene espacio para 5 automóviles en el parking. Los vehículos llegan al azar, siendo los tiempos de llegada una variable aleatoria exponencial con una media de 10 vehículos por hora. El número de personas dentro de cada coche es una variable aleatoria, $N$, donde $P(N = 1) = 0.1$, $P(N = 2) = 0.7$ y $P(N = 3) = 0.2$. La gente de los coches entra en la tienda y permanece en ella un tiempo exponencial. La duración media de la estancia en la tienda es de 10 minutos y cada persona actúa de forma independiente de todas las demás, saliendo de la tienda por separado y esperando en sus coches a los demás. Si llega un coche y la tienda está demasiado llena para que todas las personas del coche entren en ella, el coche saldrá y nadie de ese coche entrará en la tienda. Si $X(t)$ es el número de individuos en la tienda en el momento $t$, obtén la matriz de tasas corespondiente a este proceso.

-   ¿Cuál es la probabilidad a largo plazo de que la tienda esté vacía?
-   ¿Cuál es la proporción de tiempo que la estación estará completa en un periodo de 24 horas?

**Ejercicio A-10.** Un determinado equipo electrónico tiene dos componentes A y B. El tiempo hasta fallo del componente A está descrito por una función de distribución exponencial con un tiempo medio de 100 horas. El componente B tiene una vida media hasta el fallo de 200 horas y también está descrito por una distribución exponencial. Cuando uno de los componentes falla, el equipo se apaga y se realiza el mantenimiento. El tiempo de reparación del componente se distribuye exponencialmente con un tiempo medio de 5 horas si fue A el que falló y 4 horas si es B el que falla. Se considera el proceso $X(t)$ con espacio de estados $\{1, 2, 3\}$ donde el estado $1$ denota que el equipo está funcionando, $2$ denota que el componente A ha fallado, y $3$ denota que el componente B ha fallado.

-   ¿Cuál es la probabilidad a largo plazo de que el equipo funcione?
-   ¿cuál es la proporción de tiempo esperado que el sistema estará funcionando durante las próximas 500 horas?
-   Un contratista externo realiza los trabajos de reparación de los componentes cuando se produce un fallo y cobra 100 euros por hora por el tiempo más los gastos de viaje, lo que supone 500 euros más por cada visita. La empresa ha determinado que puede contratar y formar a su propio propio reparador. Si cuentan con su propio empleado para las reparaciones, le costará 40 euros por hora, tanto cuando la máquina esté en funcionamiento como cuando esté parada. Ignorando el coste de la formación inicial y la posibilidad de que un empleado contratado para los trabajos de reparación pueda hacer otras cosas mientras la máquina está en funcionamiento, ¿merece la pena económicamente contratar y formar a su propia persona?

**Ejercicio A-11.** Una pieza de automóvil necesita tres operaciones de mecanizado realizadas en una determinada secuencia. Estas operaciones son realizadas por tres máquinas. La pieza se introduce en la primera máquina, donde la operación de mecanizado dura en media 1 minuto. Una vez finalizada la operación, la pieza pasa a la máquina 2, donde el mecanizado requiere un tiempo medio de 1.2 minutos. A continuación, pasa a la máquina 3, donde la operación dura en promedio 1 minuto. No hay espacio de almacenamiento entre las dos máquinas, por tanto si la máquina 2 esta trabajando, la pieza de la máquina 1 no puede ser retirada aunque la operación en la máquina 1 se haya completado. Decimos que la máquina 1 está bloqueada en este caso. Hay un amplio suministro de piezas sin procesar disponibles de modo que la máquina 1 siempre puede procesar una nueva pieza cuando una pieza terminada se desplaza a la máquina 2. Modele este sistema como un CMTC. (Hint: Tenga en cuenta que la máquina 1 puede estar trabajar o estar bloqueada, la máquina 2 puede estar trabajando, bloqueada o inactiva, y la máquina 3 puede estar trabajando o inactiva).

-   Calcular la cantidad de tiempo esperada que la máquina 1 está bloqueada durante la primera hora, asumiendo que todas la máquinas están trabajando en el instante 0.
-   Calcule la fracción de tiempo a largo plazo en que la última máquina está trabajando en el sistema de producción.
-   Cada máquina cuesta 40 euros por hora mientras trabaja en un componente y produce un beneficio de 75 euros/hora a la pieza en la que trabaja. El valor añadido, o el coste de funcionamiento, es cero cuando la máquina está parada o bloqueada. Calcula la contribución neta de las tres máquinas por unidad de tiempo a largo plazo.

## Soluciones {#CMTCL}

**Ejercicio B-1.**


```r
# Matriz de tasas
estados <- c(0, 1, 2, 3, 4)
nestados <- length(estados)

R <- matrix(nrow = nestados, ncol = nestados, data = 0)
lambda <- 1/2
mu <- 1/72 

R[1,2] <- 2*lambda 
R[2,1] <- mu 
R[2,3] <- 2*lambda 
R[3,2] <- 2*mu 
R[3,4] <- 2*lambda 
R[4,3] <- 3*mu 
R[4,5] <- lambda
R[5,4] <- 4*mu
```

Distribución de probabilidad asociada al evento de interés:


```r
# Matriz de probabilidades de transición
Pmat<-matriz.prob.trans(R, 9, 1)
# Distribución de probabildiad buscada
Pmat[1,]
```

```
## [1] 0.0002 0.0020 0.0136 0.1547 0.8295
```

```r
# valor esperado
esperanza <- round(sum(estados*Pmat[1,]), 1)
```

Solución con simmer:


```r
replicas <- 2500
envs <- lapply(1:replicas, function(i) {
  maquinas <- mantenimiento(9, 1/2, 1/72, 2)
})
# almacenamos análisis de recursos del sistema
simresource <- as_tibble(get_mon_resources(envs))
# Almacenamos el estado final de la cola en el último instante del sistema
salida <- simresource %>%
  filter(resource == "funcionando") %>% #seleccionamos el recurso adecuado
  group_by(replication) %>%
  summarise(estado = last(system))
# Estimamos la distribución de probabilidad
distrprob <- round(table(salida$estado)/replicas, 3)
esperanza <- sum(as.numeric(names(distrprob))*distrprob)
esperanza
```

```
## [1] 0.101
```

**Ejercicio B-2.**


```r
# Matriz de tasas
estados <- c("(0, 0)", "(0, 1)", "(0, 2)", "(1, 0)", 
             "(1, 1)", "(1, 2)", "(2, 0)", "(2, 1)", "(2,2)")
nestados <- length(estados)

lambda <- 1/200
R <- matrix(nrow = nestados, ncol = nestados, data = 0)

R[2,1] <- lambda 
R[3,2] <- 2*lambda 
R[4,1] <- lambda 
R[5,2] <- lambda 
R[5,4] <- lambda 
R[6,3] <- lambda 
R[6,5] <- 2*lambda
R[7,4] <- 2*lambda
R[8,5] <- 2*lambda
R[8,7] <- lambda
R[9,6] <- 2*lambda
R[9,8] <- 2*lambda
```

Probabilidades de transición para el evento de interés:


```r
# Matriz de probabilidades de transición
Pmat<-matriz.prob.trans(R, 6, 1)
# Calculamos la probabilidad buscada
probabilidad <- Pmat[9,5] + Pmat[9,6] + Pmat[9,8] + Pmat[9,9]
probabilidad
```

```
## [1] 0.9982
```

Por tanto, la probabilidad de un fallo que le impida al avión finalizar el viaje es 0.0018.

**Ejercicio B-5.**


```r
# Matriz de tasas
nestados <- 6
lambda <- 10
mu <- 15
R <- matrix(nrow = nestados, ncol = nestados, data = 0)

R[1,2] <- lambda 
R[2,1] <- mu
R[2,3] <- lambda 
R[3,2] <- mu
R[3,4] <- lambda
R[4,3] <- mu
R[4,5] <- lambda
R[5,4] <- mu
R[5,6] <- lambda
R[6,5] <- mu
```

Obtenemos ahora la matriz $M(1)$:


```r
tiempos.ocupacion(R, 1, 1)
```

```
##        [,1]   [,2]   [,3]   [,4]   [,5]   [,6]
## [1,] 0.4451 0.2548 0.1442 0.0814 0.0465 0.0280
## [2,] 0.3821 0.2793 0.1604 0.0920 0.0535 0.0326
## [3,] 0.3246 0.2407 0.2010 0.1187 0.0710 0.0441
## [4,] 0.2746 0.2070 0.1780 0.1695 0.1046 0.0663
## [5,] 0.2356 0.1806 0.1598 0.1569 0.1624 0.1047
## [6,] 0.2124 0.1648 0.1489 0.1492 0.1571 0.1677
```

Estamos interesados en la transición $X(0) = 0$ a $X(1) = 0$ que corresponde con el elemento $[1,1]$ de la matriz obtenida. Utilizando `simmer`:


```r
# Réplicas del proceso
replicas <- 2500
envs <- lapply(1:replicas, function(i) {
  cajero <- cola.MM1K(1, 10, 15, 1, 6)
})
# almacenamos análisis de llegadas del sistema
simarrivals <- as_tibble(get_mon_arrivals(envs))
simresources <- as_tibble(get_mon_resources(envs))
# detectamos los instantes de tiempo donde el sistema esta libre y obtenemos los tiempos correspondientes
res<-c()
for(i in  1:replicas)
{
  temp <- simresources[which(simresources$replication == i), ]
  deltas <- diff(temp$time)
  free <- which(temp$system == 0)
  t_free <- deltas[free]
  res <- c(res, sum(t_free, na.rm = TRUE))
}
```

El tiempo que el sistema esta libre es de `mean(res)` horas. En este caso, la estimación obtenida si es más diferente del ressultado teórico. Podrímaos incluir un mayor número de réplicas para intentar conseguir algo más de precisión.

**Ejercicio B-6.**

Sea $X(t)$ el número de artículos en stock en el tiempo $t$, e $Y(t)$ el estado de la máquina en el tiempo $t$. El conjunto de estados de este proceso viene dado por:

$$S = \{1 = (0, 1), 2 = (1, 1), 3 = (2, 1), 4 = (3, 1), 5 = (4, 0), 6 = (3, 0)\}.$$ En la situación inicial nos encontramos en el estado $5 = (4, 0) = (X(0), Y(0))$ y estamos interesados en el tiempo en que la máquina estará después de 24 horas en los estados $1, 2, 3$ o $4$, es decir:

$$m_{51}(24) + m_{52}(24) + m_{53}(24) + m_{54}(24)$$ En esta situación tenemos que la tasa de producción es $\lambda = 60/10 = 6$ y la de demanda es $\mu = 5$, de forma que la matriz de tasas viene dada por:


```r
nestados <- 6
lambda <- 6
mu <- 5
R <- matrix(nrow = nestados, ncol = nestados, data = 0)

R[1,2] <- lambda 
R[2,1] <- mu
R[2,3] <- lambda 
R[3,2] <- mu
R[3,4] <- lambda
R[4,3] <- mu
R[4,5] <- lambda
R[5,6] <- mu
R[6,3] <- mu
```

Obtenemos ahora la matriz $M(24)$:


```r
mmat<-tiempos.ocupacion(R, 24, 1)
mmat
```

```
##        [,1]   [,2]   [,3]   [,4]   [,5]   [,6]
## [1,] 4.0004 4.6322 5.4284 2.9496 3.5097 3.4798
## [2,] 3.8602 4.6639 5.4664 2.9703 3.5345 3.5047
## [3,] 3.7697 4.5553 5.5361 3.0084 3.5802 3.5503
## [4,] 3.7207 4.4965 5.4656 3.0608 3.6431 3.6132
## [5,] 3.7063 4.4793 5.4448 2.9586 3.7204 3.6906
## [6,] 3.7380 4.5173 5.4905 2.9835 3.5503 3.7204
```

La cantidad buscada viene dada por:


```r
sum(mmat[5, 1:4])
```

```
## [1] 16.589
```

**Ejercicio B-9.**


```r
maquinas <- 4
estados <- maquinas + 1
operarios <- 2
lambda <- 1/2
mu <- 1/72
lambdas <- pmin(maquinas - (0:(maquinas-1)), operarios)*lambda
mus <- (1:(maquinas-1))*mu
# Debemos quitar el último elemento de lambda y el primero de mu para usar la función definida
probs <- distr.lim.nm(estados, lambdas, mus)
```

```
## Warning in prl/prm: longitud de objeto mayor no es múltiplo de la longitud de uno menor
```

```r
probs
```

```
## [1] 1.540618e-05 1.109245e-03 3.993283e-02 9.583879e-01 5.546226e-04
```

Así, a largo plazo, todas las máquinas funcionan el 89,6% del tiempo ($p_4 = p[5]$). Ambos operarios están ocupados siempre que el sistema está en el estado $0$, $1$ o $2$. Por lo tanto, la fracción de tiempo a largo plazo en que ambos reparadores están ocupados viene dada por $p_0 + p_1 + p_2$:


```r
sum(probs[1:3])
```

```
## [1] 0.04105748
```

**Ejercicio B-10.**


```r
maxcola <- 5
estados <- maxcola + 1
lambda <- 10
mu <- 15
lambdas <- rep(lambda, maxcola)
mus <- rep(mu, maxcola)
# Distribución límite
probs <- distr.lim.nm(estados, lambdas, mus)
probs
```

```
## [1] 0.36541353 0.24360902 0.16240602 0.10827068 0.07218045 0.04812030
```

**Ejercicio B-11.**

Recordemos que el espacio de estados es $\{1, 2,...,9\}$ y nos intersa obtener la probabilidad del sujconjunto $\{1, 2, 3, 4, 7\}$. Obtenemos en primer lugar la matriz de tasas:


```r
# Estados del sistema
nestados <- 9
lambda <- 1/20
# Matriz de tasas
R <- matrix(nrow = nestados, ncol = nestados, data = 0)
R[2,1] <- lambda 
R[3,2] <- 2*lambda 
R[4,1] <- lambda 
R[5,2] <- lambda 
R[5,4] <- lambda 
R[6,3] <- lambda 
R[6,5] <- 2*lambda
R[7,4] <- 2*lambda
R[8,5] <- 2*lambda
R[8,7] <- lambda
R[9,6] <- 2*lambda
R[9,8] <- 2*lambda
# Resolución  de las ecuaciones de balance
ps <- distr.lim.general(R)
ps
```

```
## [1] 1 0 0 0 0 0 0 0 0
```

**Ejercicio B-12.**


```r
# Estados del sistema
nestados <- 3
# Matriz de tasas
R <- matrix(nrow = nestados, ncol = nestados, data = 0)
R[1,1] <- 0 
R[1,2] <- 1/4
R[1,3] <- 1/4
R[2,1] <- 3/4
R[2,2] <- 0
R[2,3] <- 1/4
R[3,1] <- 1/2
R[3,2] <- 1/6
R[3,3] <- 0
# Resolución  de las ecuaciones de balance
ps <- distr.lim.general(R)
# Expresando en porcentajes
round(100*ps, 2)
```

```
## [1] 54.55 18.18 27.27
```
