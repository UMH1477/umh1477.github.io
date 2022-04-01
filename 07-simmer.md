---
editor_options: 
  markdown: 
    wrap: sentence
---

# Simulación DES con simmer {#simmerunidad}

## Simulación

Algunos sistemas del mundo real contienen tal complejidad que es inviable representarlos íntegramente y de modo preciso a través de modelos analíticos; para estudiarlos sin embargo, podemos recurrir a la **simulación**.
@shannon define simulación como el proceso de diseñar un modelo de un sistema real y realizar experimentos con este modelo, con el propósito de comprender su comportamiento o de evaluar diversas estrategias para que el sistema opere (dentro de los límites impuestos por un criterio o conjunto de criterios).
Dependiendo de la naturaleza del sistema a representar, hay varios tipos de simulación.
Una taxonomía habitual común para clasificar los problemas de simulación lo hace considerando tres dimensiones [@lawkelton]:

1.  Variación aleatoria: **determinística vs. estocástica**, es decir, bien se utilizan fórmulas determinísticas o bien se incorpora variabilidad estocástica.
2.  Variación en el tiempo: **estática vs. dinámica**, esto es, se simula un proceso estático que no varía a lo largo del tiempo, o uno dinámico que sí lo hace.
3.  Variación en la medida: **continua vs. discreta**, es decir, el resultado o *output* medible del proceso varía de modo continuo o lo hace a saltos discretos.

## Simulación DES

La simulación de eventos discretos (Discrete Event Simulation -DES-) es una técnica específica para modelar sistemas **estocásticos** que evolucionan en el tiempo (son **dinámicos**) a saltos **discretos**, esto es, su estado cambia de forma discreta en instantes concretos a lo largo de instantes de tiempo que se secuencian según alguna ley de probabilidad.

Son ejemplos de procesos DES las colas de espera, los productos en una cadena de producción, e incluso los objetos digitales que se mueven en una red social.
Su naturaleza discreta permite describir su comportamiento en términos de **eventos**, esto es, de una ocurrencia instantánea que puede cambiar el estado del sistema, mientras que entre eventos consecutivos todas las variables de estado van a permanecer invariables.

Las aplicaciones DES son muy numerosas: sistema de fabricación, ingeniería de la construcción, gestión de proyectos, logística, sistemas de transporte, procesos de negocios, salud, redes de comunicaciones, ... [@banks].
La simulación de tales sistemas nos proporciona información sobre el riesgo, eficiencia y efectividad de los procesos, e incluso nos permite estudiar los efectos de introducir cambios en el sistema.
En servicios públicos nos permite estudiar cuellos de botella en colas, optimizar flujos de pacientes en los hospitales, testar la robustez de una cadena de producción o predecir el funcionamiento de un nuevo protocolo o configuración en una red de telecomunicaciones.

Existen tres aproximaciones básicas para proceder en la simulación DES [@banks]:

-   Orientada a la actividad (*activity-oriented*): el modelo consiste en secuencias de actividades que esperan a ser ejecutadas dependiendo de ciertas condiciones; el reloj de simulación avanza a incrementos fijos de tiempo. En cada paso se escanea toda la lista de actividades y se verifican sus condiciones de ejecución. Este modo de simular es demasiado sensible a la elección del incremento de tiempos, dada la aleatoriedad de los tiempos en que suceden los eventos.
-   Orientada a los eventos (*event-oriented*): mantiene una lista de eventos programados y ordenados por el tiempo en el que van a ocurrir. La simulación consiste en saltar de evento a evento, ejecutando secuencialmente las rutinas asociadas.
-   Orientada a los procesos (*process-oriented*): refina la simulación orientada a eventos añadiendo procesos que interactúan y cuya activación es desencadenada por eventos. En este caso, el modelizador define un conjunto de procesos que corresponden a entidades u objetos del sistema real y su ciclo de vida.

## Software

La librería **simmer** [@R-simmer] es un paquete de R para simulación DES que permite una **modelización orientada a procesos** de alto nivel.
Además, explota el novedoso concepto de *trayectoria*: un camino común (o modo de comportamiento único) en el modelo de simulación para entidades (procesos) del mismo tipo (equivalentes).
Aprovecha además la definición de flujos de trabajo en cadena (*pipe*) del paquete **magrittr** [@R-magrittr].

La librería **simmer** se empezó a desarrollar en 2014 para resolver un problema de optimización de facilidades en servicios sanitarios, basada en la simulación de eventos discretos *DES*.
Está implementada en C++ y de ahí su eficiencia.

´simmer\` no es la única librería de simulación de procesos en R, donde contamos también con las librerías:

-   `SpaDES` [@spades], que se centra en modelos discretos espaciales;
-   `queuecomputer` [@R-queuecomputer] implementa un método eficiente para simular colas con llegadas y tiempos de servicio arbitrarios;
-   `queueing` [@R-queueing] contiene múltiples funciones para analizar sistemas de colas.

Más allá del lenguaje R, los competidores directos de `simmer` son `SimPy` [@simpy] y `SimJulia` [@simjulia], construidos, respectivamente, bajo los lenguajes Python y Julia.

Instalamos y cargamos pues, las librerías vinculadas a `simmer` y otras generales básicas en R:


```r
library(simmer)
library(simmer.bricks)
library(simmer.plot)
library(parallel)
library(dplyr)
library(tidyverse)
```

## Conceptos clave

Para entender la Simulación de Eventos Discretos (DES), es preciso controlar cierta terminología específica, que se muestra a continuación:

-   **Recurso (*resource*)**.
    Se trata de una entidad pasiva que no se mueve pero proporciona un servicio o realiza una actividad dirigida a las llegadas que se producen en el sistema.
    Todo recurso en `simmer` contiene dos elementos auto-gestionados:

    -   Servidor, que representa el recurso que da servicio o realiza la actividad, con una determinada capacidad y que puede ser accedido (*seized*) y abandonado (*released*).
    -   Cola: una cola priorizada de cierto tamaño (puede ser infinita), que se llena cuando el servidor está a plena capacidad.

-   **Gestor (*manager*)**.
    Es una entidad activa, esto es, se trata de un proceso que tiene la habilidad de reajustar las propiedades de un recurso (como su capacidad y tamaño de la cola) a medida que transcurre el tiempo (*run-time*).

-   **Fuente (*source*)** es el proceso responsable de generar nuevas llegadas según un patrón de tiempos entre llegadas, y de integrarlas en el modelo de simulación.

-   **Llegada (*arrival*)**.
    Es un proceso capaz de interaccionar con los recursos o con otras entidades del modelo de simulación.
    Puede tener atributos y valores de priorización asociados, y en general, tiene un tiempo de vida limitado.
    Tras su creación, cada llegada es integrada en una trayectoria dada.

-   **Trayectoria (*trajectory*)**.
    Se trata de la secuenciación de actividades que definen el camino a seguir de cada una de las llegadas que acceden a ella.
    El modelo de simulación se representa por un conjunto de trayectorias.

-   **Actividad (*activity*)** es la unidad individual de acción que permite que las llegadas interactúen con los recursos y otras entidades, realicen rutinas mientras están en el sistema, retrocedan y avancen a lo largo de la trayectoria de forma dinámica, etc.

Todos los procesos son susceptibles de ser representados a través de diagramas de flujo.
Es conveniente siempre representarlos, con el fin de ordenar y mostrar con claridad los caminos y secuenciaciones involucradas.
Para crear diagramas de flujo disponemos de la simbología estándar [ANSI para diagramas de flujo](https://docs.google.com/document/d/1F7urSCA-82umRB8a85P67yOiS35HNmbuL1FpLjemQ8Q/edit?usp=sharing)).

## Entornos y trayectorias

La simulación con `simmer`, aunque se puede basar en los eventos, funciona mejor orientada a los procesos, esto es, enfocada a la identificación de fuentes y procesos y a las interacciones entre ellos.
Utiliza dos elementos básicos:

-   El **entorno de simulación** se almacena en un objeto simmer definido por el comando `env()`, gestiona los recursos (*resources*) y las fuentes de llegadas (*generators*), controla la ejecución de la simulación y contiene los resultados de la simulación.
-   Las **trayectorias**, definidas con el comando `trajectory()`, que contienen las secuencias de acciones o actividades que van a experimentar las llegadas. Cada actividad viene descrita por un verbo que representa una funcionalidad concreta (bloque funcional). El conjunto de acciones disponibles en `simmer` lo estudiaremos en la sección [Acciones](@acciones).

Simular con `simmer` consiste, simplemente, en construir un entorno de simulación en el que se añaden recursos que dan servicios y se generan llegadas que acceden a trayectorias en las que desarrollan ciertas actividades y son atendidas por los recursos.
Toda la sintaxis se encadena con el comando pipe `%>%`.

La Figura \@ref(fig:simmerentorno) ilustra de un modo sencillo la dinámica de simulación con `simmer` a partir de entornos y trayectorias: el simulador es inicializado con `simmer` y monitorizado con `monitor`.
Cada simulador contiene entidades constituidas por procesos y recursos.
A los procesos acceden las llegadas, provenientes de fuentes y reguladas por gestores, que realizan una serie de actividades que definen trayectorias y son ejecutadas por los recursos.
Así mismo, los gestores pueden interaccionar con los recursos.

\begin{figure}

{\centering \includegraphics[width=0.8\linewidth]{images/simmer_entorno} 

}

\caption{Figura 2. Entorno de simulación en `simmer` [Fte: IBiDat](https://ibidat.es/portfolio-items/simmer-discrete-event-simulation-for-r/)}(\#fig:simmerentorno)
\end{figure}

Aunque las trayectorias se definen de modo independiente a los entornos de simulación, se recomienda definir siempre el entorno en primer lugar (pues nombra recursos y llegadas), `env=simmer()`, para luego definir las trayectorias y completar el entorno con los recursos y las fuentes.
Hay que tener en cuenta que las trayectorias y los entornos van a usar elementos comunes, con lo que la sintaxis ha se ser construida y revisada en paralelo.

Para hacer correr la simulación utilizamos el comando `run()`, con el que podemos especificar cuándo parar.
El comando `reset()` nos permite resetear una simulación.

Las trajectorias se pueden seccionar con el operador de selección `[]`, juntar con `join()` y modificar con el operador `=`. También podemos conocer el número de actividades con `get_n_activities(traj)`.

Hay muchas actividades disponibles en simmer para incorporar en trayectorias y que presentaremos categorizadas más adelante, según su funcionalidad.

Veamos un ejemplo de un modelo de simulación sencillo en el que llegan clientes a una tienda cada minuto (durante 3 minutos), miran los productos durante 5 minutos y se van.


```r
env=simmer()
# Se define la trayectoria "tienda" con la actividad que realizan los clientes
tienda=trajectory() %>%
  # Lanza un mensaje de aviso de llegada
  log_("Llega a la tienda") %>%
  # mira productos 5min
  timeout(5) %>% 
  # Lanza un mensaje de aviso de llegada
  log_("Sale de la tienda")
# se lanza el entorno de simulación incluyendo el generador de llegadas 
# (clientes), que aparecen en tres instantes de tiempo,
# y que son dirigidos a la trayectoria "tienda"
env=env %>%
  add_generator("cliente", tienda,at(1,2,3)) %>%
  # se muestran los resultados en pantalla
  print() %>%
  # se lanza el sistema
  run()
```

```
## simmer environment: anonymous | now: 0 | next: 0
## { Monitor: in memory }
## { Source: cliente | monitored: 1 | n_generated: 0 }
## 1: cliente0: Llega a la tienda
## 2: cliente1: Llega a la tienda
## 3: cliente2: Llega a la tienda
## 6: cliente0: Sale de la tienda
## 7: cliente1: Sale de la tienda
## 8: cliente2: Sale de la tienda
```

Al visualizar el output, apreciamos que llegan tres clientes en los instantes 1,2,3.
Cada cliente permanece en la tienda durante 5 minutos inspeccionando la mercancía, y después sale en los instantes 6, 7 y 8 respectivamente.
En las actividades y generadores de llegadas hemos utilizado parámetros fijos, pero los podemos hacer aleatorios.
Adaptemos el ejemplo anterior a tiempos aleatorios uniformes entre llegadas y tiempos de permanencia aleatorios normales.


```r
env=simmer()
tienda=trajectory() %>%
  log_("Llega a la tienda") %>%
  # Cada cliente permanece en la tienda mirando productos aprox. 5min
  timeout(function() rnorm(1,5,1)) %>% 
  log_("Sale de la tienda")

env=env %>%
  # el tiempo entre llegadas de los clientes es aleatorio entre 0 y 2 min.
  add_generator("cliente", tienda,function() runif(1,0,2)) %>% 
  print() %>%
  # simulamos hasta el instante 10
  run(10) 
```

```
## simmer environment: anonymous | now: 0 | next: 0
## { Monitor: in memory }
## { Source: cliente | monitored: 1 | n_generated: 0 }
## 1.20152: cliente0: Llega a la tienda
## 1.51594: cliente1: Llega a la tienda
## 2.51149: cliente2: Llega a la tienda
## 3.76426: cliente0: Sale de la tienda
## 4.05654: cliente3: Llega a la tienda
## 4.12502: cliente4: Llega a la tienda
## 4.51636: cliente5: Llega a la tienda
## 5.29376: cliente6: Llega a la tienda
## 5.96187: cliente1: Sale de la tienda
## 6.65052: cliente7: Llega a la tienda
## 8.58992: cliente3: Sale de la tienda
## 8.6116: cliente8: Llega a la tienda
## 8.65991: cliente2: Sale de la tienda
## 8.88082: cliente4: Sale de la tienda
## 9.67203: cliente9: Llega a la tienda
## 9.73449: cliente10: Llega a la tienda
```

Ahora vemos en la primera columna los instantes de tiempo en que llegan y se van los clientes.

## Monitoreo

Al ejecutar un entorno de simulación, si no incluimos ninguna opción de visualización de mensajes en la trayectoria, obtenemos en pantalla un resumen escrito básico sobre recursos y llegadas.


```r
trayectoria=trajectory() %>%
   timeout(3)

simmer() %>%
  add_generator("llegada",trayectoria,at(0,1,2)) %>%
  run() 
```

```
## simmer environment: anonymous | now: 5 | next: 
## { Monitor: in memory }
## { Source: llegada | monitored: 1 | n_generated: 3 }
```

Al añadir en la ejecución del entorno el comando `print()` (antes de `run()`), se monitorizan los eventos que se van sucediendo en la simulación y dónde se almacenan (Monitor).


```r
simmer() %>%
  add_generator("llegada",trayectoria,at(0,1,2)) %>%
  print() %>%
  run() 
```

```
## simmer environment: anonymous | now: 0 | next: 0
## { Monitor: in memory }
## { Source: llegada | monitored: 1 | n_generated: 0 }
```

```
## simmer environment: anonymous | now: 5 | next: 
## { Monitor: in memory }
## { Source: llegada | monitored: 1 | n_generated: 3 }
```

El comando `invisible` (después de `run()`) depura la salida para no visualizar nada (salvo que hayamos incorporado mensajes en las trayectorias).


```r
simmer() %>%
  add_generator("llegada",trayectoria,at(0,1,2)) %>%
  run() %>%
  invisible
```

Si queremos visualizar mensajes específicos vinculados a las llegadas y actividades conforme se suceden, habremos de incluir mensajes en las trayectorias a través del comando `log_()`.


```r
env=simmer()
trayectoria=trajectory() %>%
  log_("Llegada contabilizada.") %>%
  timeout(3)

env=env %>%
  add_generator("llegada",trayectoria,function() rexp(1,1/2)) %>%
  run(5) %>%
  invisible
```

```
## 0.825735: llegada0: Llegada contabilizada.
## 1.07574: llegada1: Llegada contabilizada.
## 1.79639: llegada2: Llegada contabilizada.
## 3.3158: llegada3: Llegada contabilizada.
## 4.3945: llegada4: Llegada contabilizada.
## 4.93402: llegada5: Llegada contabilizada.
```

Una vez lanzamos un sistema de simulación, disponemos de diversos métodos para extraer información de él y monitorizarla por ejemplo con el comando `log_()`:

-   `now()` el tiempo de simulación actual
-   `peek(n)` instantes de tiempo en los que acontecerán los siguientes $n$ eventos
-   `get_*()` para obtener información de recursos, atributos y fuentes: `attribute`, `capacity`, `global`, `name`, `n_activities`, `n_generated`, `prioritization`, `queue_count`, `queue_size`, `resources`, `seized`, `server_count`, `sources`, `trajectory`.


```r
env %>%
    peek(3)
```

```
## [1] 6.315802 6.842251 6.842251
```

Con el comando `stepn()` podemos hacer correr el sistema durante una única simulación adicional, que no se almacena en el objeto de simulación salvo que lo asignemos a él.


```r
env=env %>%
  stepn() %>%
  print()
```

```
## simmer environment: anonymous | now: 6.3158017488099 | next: 6.84225121299232
## { Monitor: in memory }
## { Source: llegada | monitored: 1 | n_generated: 7 }
```

Los comandos para monitorizar se pueden aplicar a un único entorno de simulación o a una lista de entornos, y el objeto de retorno es siempre un data frame, incluso si no se encuentran datos.
Cada entorno de simulación procesado se trata como una replicación diferente, y se identifica como tal en una columna numérica denominada *replication* en el dataframe de retorno, con los índices de los entornos como valores.
Recuperamos llegadas, recursos y atributos del sistema con los comandos:

-   `get_mon_arrivals()` devuelve la información temporal por llegada: nombre (*name*) de la llegada, tiempo de llegada (*start_time*), tiempo de salida (*end_time*), tiempo en la actividad sin estar en la cola (*activity_time*) y una etiqueta *finished* que indica si la llegada finalizó sus actividades.
    Por defecto esta información se refiere al tiempo de vida completo de las llegadas, pero se puede obtener por recurso con el argumento `per_resource=TRUE`.

-   `get_mon_resources()` devuelve los cambios de estado en los recursos: nombre del recurso (*resource*), instante (*time*) del evento que desencadenó el cambio de estado, contador de unidades en servicio (*server*), contador de unidades en la cola (*queue*), capacidad (*capacity*), tamaño de la cola (*queue_size*), contador del sistema (*system=server+queue*), y límite del sistema (*limit=capacity+queue_size*).

-   `get_mon_atributes()` devuelve los cambios de estado en los atributos: nombre (*name*) del atributo, instante (*time*) del evento que desencadenó el cambio de estado, nombre (*key*) que identifica el atributo y valor (*value*).

```{=html}
<!-- -->
```
    llegadas=get_mon_arrivals(env) 
    llegadas
    atributos=get_mon_attributes(env)
    atributos
    recursos=get_mon_resources(env)
    recursos

Ejemplifiquemos su funcionamiento sobre el sistema sencillo de la tienda que ya vimos anteriormente, con tiempos para mirar al llegar a la tienda, seguidos de atención por dependientes.


```r
env=simmer()
tienda=trajectory() %>%
  log_(function()  "Llega a la tienda") %>%
  timeout(function() rnorm(1,5,1)) %>% # mira productos aprox. 5min
  seize("dependiente",1) %>%
  log_("Es atendido") %>%
  timeout(function() rnorm(1,10,1)) %>%
  release("dependiente",1)%>%
  log_(function() "Sale de la tienda")

env=env %>%
  add_generator("cliente", tienda,function() runif(1,0,5)) %>% # tiempo entre llegadas 
  add_resource("dependiente",2) %>% # hay dos dependientes
  print() %>%
  run(20) # simulamos hasta ese instante
```

```
## simmer environment: anonymous | now: 0 | next: 0
## { Monitor: in memory }
## { Resource: dependiente | monitored: TRUE | server status: 0(2) | queue status: 0(Inf) }
## { Source: cliente | monitored: 1 | n_generated: 0 }
## 0.873379: cliente0: Llega a la tienda
## 3.53125: cliente1: Llega a la tienda
## 4.55214: cliente2: Llega a la tienda
## 5.85743: cliente0: Es atendido
## 6.32317: cliente3: Llega a la tienda
## 9.09458: cliente1: Es atendido
## 9.30449: cliente4: Llega a la tienda
## 10.6975: cliente5: Llega a la tienda
## 10.8378: cliente6: Llega a la tienda
## 10.9381: cliente7: Llega a la tienda
## 15.2235: cliente8: Llega a la tienda
## 16.0339: cliente0: Sale de la tienda
## 16.0339: cliente3: Es atendido
## 18.2267: cliente9: Llega a la tienda
## 19.2013: cliente1: Sale de la tienda
## 19.2013: cliente2: Es atendido
```

```r
llegadas=get_mon_arrivals(env) 
llegadas
```

```
##       name start_time end_time activity_time finished replication
## 1 cliente0  0.8733795 16.03392      15.16054     TRUE           1
## 2 cliente1  3.5312472 19.20127      15.67002     TRUE           1
```

```r
atributos=get_mon_attributes(env)
atributos
```

```
## [1] time  name  key   value
## <0 rows> (or 0-length row.names)
```

```r
recursos=get_mon_resources(env)
recursos
```

```
##       resource      time server queue capacity queue_size system limit replication
## 1  dependiente  5.857429      1     0        2        Inf      1   Inf           1
## 2  dependiente  9.094584      2     0        2        Inf      2   Inf           1
## 3  dependiente 10.450857      2     1        2        Inf      3   Inf           1
## 4  dependiente 10.487502      2     2        2        Inf      4   Inf           1
## 5  dependiente 14.170490      2     3        2        Inf      5   Inf           1
## 6  dependiente 15.524331      2     4        2        Inf      6   Inf           1
## 7  dependiente 15.612112      2     5        2        Inf      7   Inf           1
## 8  dependiente 15.645231      2     6        2        Inf      8   Inf           1
## 9  dependiente 16.033918      2     5        2        Inf      7   Inf           1
## 10 dependiente 19.201269      2     4        2        Inf      6   Inf           1
## 11 dependiente 19.584398      2     5        2        Inf      7   Inf           1
```

También podemos derivar el output de un entorno de simulación a ficheros 'csv' (con `monitor_csv`) o de otro tipo, como '.txt' especificando los delimitadores (con `monitor_delim`), con todas las llegadas (*arrivals*), atributos (*attributes*), salidas (*releases*) y recursos (*resources*), almacenándolas en el directorio temporal `tempdir()` o en nuestro propio directorio de trabajo.


```r
mon <- monitor_csv(path=tempdir()) # tempdir() lo podemos sustituir por nuestro directorio de trabajo
mon   # nos muestra la ubicación y nombre de los ficheros
```

```
## simmer monitor: to disk (delimited files)
## { arrivals: /var/folders/m7/28kglwg91xjbzq3fpmfgdw4c0000gn/T//RtmpUTKE76/file8f282262cef5_arrivals.csv }
## { releases: /var/folders/m7/28kglwg91xjbzq3fpmfgdw4c0000gn/T//RtmpUTKE76/file8f282262cef5_releases.csv }
## { attributes: /var/folders/m7/28kglwg91xjbzq3fpmfgdw4c0000gn/T//RtmpUTKE76/file8f282262cef5_attributes.csv }
## { resources: /var/folders/m7/28kglwg91xjbzq3fpmfgdw4c0000gn/T//RtmpUTKE76/file8f282262cef5_resources.csv }
```

```r
env_mon=simmer(mon=mon)
tienda=trajectory() %>%
  log_(function()  "Llega a la tienda") %>%
  timeout(function() rnorm(1,5,1)) %>% # mira productos aprox. 5min
  seize("dependiente",1) %>%
  log_("Es atendido") %>%
  timeout(function() rnorm(1,10,1)) %>%
  release("dependiente",1)%>%
  log_(function() "Sale de la tienda")

env_mon=env_mon %>%
  add_generator("cliente", tienda,function() runif(1,0,5)) %>% # tiempo entre llegadas 
  add_resource("dependiente",2) %>% # hay dos dependientes
  print() %>%
  run(20) # simulamos hasta ese instante
```

```
## simmer environment: anonymous | now: 0 | next: 0
## { Monitor: to disk (delimited files) }
##   { arrivals: /var/folders/m7/28kglwg91xjbzq3fpmfgdw4c0000gn/T//RtmpUTKE76/file8f282262cef5_arrivals.csv }
##   { releases: /var/folders/m7/28kglwg91xjbzq3fpmfgdw4c0000gn/T//RtmpUTKE76/file8f282262cef5_releases.csv }
##   { attributes: /var/folders/m7/28kglwg91xjbzq3fpmfgdw4c0000gn/T//RtmpUTKE76/file8f282262cef5_attributes.csv }
##   { resources: /var/folders/m7/28kglwg91xjbzq3fpmfgdw4c0000gn/T//RtmpUTKE76/file8f282262cef5_resources.csv }
## { Resource: dependiente | monitored: TRUE | server status: 0(2) | queue status: 0(Inf) }
## { Source: cliente | monitored: 1 | n_generated: 0 }
## 0.745177: cliente0: Llega a la tienda
## 3.33796: cliente1: Llega a la tienda
## 4.54453: cliente2: Llega a la tienda
## 4.68431: cliente3: Llega a la tienda
## 6.76511: cliente0: Es atendido
## 8.45616: cliente1: Es atendido
## 8.75457: cliente4: Llega a la tienda
## 11.3135: cliente5: Llega a la tienda
## 15.6846: cliente6: Llega a la tienda
## 16.559: cliente0: Sale de la tienda
## 16.559: cliente3: Es atendido
## 18.4753: cliente1: Sale de la tienda
## 18.4753: cliente2: Es atendido
```

para a continuación cargar los datos de estos ficheros y trabajar sobre ellos,


```r
llegadas=read.csv(mon$handlers$arrivals)
llegadas
```

```
##       name start_time end_time activity_time finished
## 1 cliente0  0.7451773 16.55902      15.81385        1
## 2 cliente1  3.3379605 18.47533      15.13737        1
```

```r
# que contiene la misma información (salvo "replication") que
get_mon_arrivals(env_mon)
```

```
##       name start_time end_time activity_time finished replication
## 1 cliente0  0.7451773 16.55902      15.81385        1           1
## 2 cliente1  3.3379605 18.47533      15.13737        1           1
```

Una vez extraída la información simulada para monitorizar el proceso y realizar análisis, con el paquete `simmer.plot` (Ucar and Smeets, 2019b) tenemos métodos de graficado para visualizar rápidamente el uso de un recurso o una llegada a lo largo del tiempo.

Para las **llegadas** se pintan gráficos de líneas, con tres opciones o métricas posibles:

-   *activity_time* es la cantidad de tiempo que consume en actividades cada llegada;
-   *flow_time* es la cantidad de tiempo que consume cada llegada en el sistema, y se calcula con: '*flow = end_time - start_time*'.
-   *waiting_time* es la cantidad de tiempo que una llegada espera a ser atendida o ejecutada, y se calcula con: '*waiting_time = flow_time - activity_time*'.


```r
arrivals=get_mon_arrivals(env_mon)
plot(arrivals, metric="waiting_time")
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : span too small.
## fewer data values than degrees of freedom.
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : at 16.549
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : radius 9.1806e-05
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : all data on
## boundary of neighborhood. make span bigger
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : pseudoinverse used
## at 16.549
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : neighborhood
## radius 0.0095815
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : reciprocal
## condition number 1
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : at 18.485
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : radius 9.1806e-05
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : all data on
## boundary of neighborhood. make span bigger
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : There are other
## near singularities as well. 9.1806e-05
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : zero-width
## neighborhood. make span bigger

## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : zero-width
## neighborhood. make span bigger
```

```
## Warning: Computation failed in `stat_smooth()`:
## NA/NaN/Inf en llamada a una función externa (arg 5)
```



\begin{center}\includegraphics[width=0.95\linewidth]{07-simmer_files/figure-latex/unnamed-chunk-13-1} \end{center}

```r
plot(arrivals, metric="activity_time")
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : span too small.
## fewer data values than degrees of freedom.
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : at 16.549
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : radius 9.1806e-05
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : all data on
## boundary of neighborhood. make span bigger
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : pseudoinverse used
## at 16.549
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : neighborhood
## radius 0.0095815
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : reciprocal
## condition number 1
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : at 18.485
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : radius 9.1806e-05
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : all data on
## boundary of neighborhood. make span bigger
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : There are other
## near singularities as well. 9.1806e-05
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : zero-width
## neighborhood. make span bigger

## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : zero-width
## neighborhood. make span bigger
```

```
## Warning: Computation failed in `stat_smooth()`:
## NA/NaN/Inf en llamada a una función externa (arg 5)
```



\begin{center}\includegraphics[width=0.95\linewidth]{07-simmer_files/figure-latex/unnamed-chunk-13-2} \end{center}

```r
plot(arrivals, metric="flow_time")
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : span too small.
## fewer data values than degrees of freedom.
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : at 16.549
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : radius 9.1806e-05
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : all data on
## boundary of neighborhood. make span bigger
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : pseudoinverse used
## at 16.549
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : neighborhood
## radius 0.0095815
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : reciprocal
## condition number 1
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : at 18.485
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : radius 9.1806e-05
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : all data on
## boundary of neighborhood. make span bigger
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : There are other
## near singularities as well. 9.1806e-05
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : zero-width
## neighborhood. make span bigger

## Warning in simpleLoess(y, x, w, span, degree = degree, parametric = parametric, : zero-width
## neighborhood. make span bigger
```

```
## Warning: Computation failed in `stat_smooth()`:
## NA/NaN/Inf en llamada a una función externa (arg 5)
```



\begin{center}\includegraphics[width=0.95\linewidth]{07-simmer_files/figure-latex/unnamed-chunk-13-3} \end{center}

Para los **recursos**, tenemos dos métricas de graficado:

-   *usage* muestra un gráfico de líneas con el tiempo promedio de uso (acumulado) de cada recurso, replication e item (por defecto queue, server y system, que es la suma de servidor y cola). Si *steps=TRUE*, se pinta un gráfico de escaleras con los valores instantáneos.
-   *utilization* muestra un gráfico de barras con el promedio de uso del recurso (tiempo total en uso, dividido por el tiempo total de simulación). Para replicaciones múltiples, la barra representa la mediana y las barras de error los cuartiles. Si se proporciona una única replicación, la barra y la barra de error coinciden.



Teniendo en cuenta que `get_mon` proporciona objetos data.frame, podemos utilizar cualquier otro tipo de gráfico específico y personalizado con las funciones gráficas habituales de R, y en particular de 'ggplot2'.


```r
arrivals=get_mon_arrivals(env_mon)
ggplot(arrivals,aes(x=activity_time))+
  geom_histogram(fill="Skyblue",color="blue")
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```



\begin{center}\includegraphics[width=0.95\linewidth]{07-simmer_files/figure-latex/unnamed-chunk-15-1} \end{center}

## Fuentes de llegadas

Una fuente de llegadas, clientes o productos de un sistema de simulación es definida con tres parámetros principales:

-   *name_prefix*, el nombre con el que identificamos cada llegada que se genera
-   *trajectory*, la trayectoria a la que accede
-   *distribution*, la distribución de los tiempos entre llegadas.

Hay dos tipos de fuentes: generadores y fuentes de datos (*dataframes*), a las que accedemos respectivamente con los comandos `add_generator()` y `add_dataframe()`.

-   **Generadores**, proporcionados con el comando [`add_generator()`](https://r-simmer.org/reference/generators.html), que genera tiempos dinámicos entre llegadas a partir de una función (distribución) que define el usuario.

Ejemplifiquemos las posibilidades de generación de tiempos de llegadas con el ejemplo anterior que ya realizamos, en el que al entrar cada cliente, pasa 5 minutos en el sistema y luego se marcha.


```r
env=simmer()
# defino la función de los tiempos entre llegadas, U(0,2)
distr <- function() runif(1, 0, 2)

# definimos laa acciones que hace el cliente en la tienda
tienda <- trajectory() %>%
  timeout(5)

env %>%
  # los tiempos entre llegadas se simulan de "distr"
  add_generator("llegada_random", tienda, distr) %>%
  # las llegadas ocurren en los instantes 0, 1, 10, 30, 40 y 43
  add_generator("llegada_at", tienda, at(0,1,10,30,40,43)) %>%
  # los tiempos entre llegadas se simulan de "distr", empezando en el instante 1
  add_generator("llegada_from", tienda, from(1, distr)) %>%
  # los tiempos entre llegadas se simulan de "distr", acabando en el instante 5
  add_generator("llegada_to", tienda, to(5, distr)) %>%
  # los tiempos entre llegadas se simulan de "distr", empezando en 1 y acabando en 5
  add_generator("llegada_from_to", tienda, from_to(1, 5, distr, every=4)) %>%
  run(15) %>% 
  # monitorizamos todas las llegadas
  get_mon_arrivals()
```

```
##                name start_time  end_time activity_time finished replication
## 1       llegada_at0   0.000000  5.000000             5     TRUE           1
## 2       llegada_to0   0.505994  5.505994             5     TRUE           1
## 3       llegada_at1   1.000000  6.000000             5     TRUE           1
## 4     llegada_from0   1.000000  6.000000             5     TRUE           1
## 5  llegada_from_to0   1.000000  6.000000             5     TRUE           1
## 6   llegada_random0   1.133789  6.133789             5     TRUE           1
## 7  llegada_from_to1   1.497077  6.497077             5     TRUE           1
## 8   llegada_random1   1.939551  6.939551             5     TRUE           1
## 9   llegada_random2   2.178522  7.178522             5     TRUE           1
## 10      llegada_to1   2.343600  7.343600             5     TRUE           1
## 11  llegada_random3   2.567912  7.567912             5     TRUE           1
## 12      llegada_to2   2.672739  7.672739             5     TRUE           1
## 13    llegada_from1   2.734700  7.734700             5     TRUE           1
## 14 llegada_from_to2   3.036338  8.036338             5     TRUE           1
## 15  llegada_random4   3.894325  8.894325             5     TRUE           1
## 16 llegada_from_to3   4.141093  9.141093             5     TRUE           1
## 17      llegada_to3   4.385889  9.385889             5     TRUE           1
## 18    llegada_from2   4.587793  9.587793             5     TRUE           1
## 19    llegada_from3   4.677028  9.677028             5     TRUE           1
## 20      llegada_to4   4.875325  9.875325             5     TRUE           1
## 21 llegada_from_to4   5.000000 10.000000             5     TRUE           1
## 22  llegada_random5   5.048456 10.048456             5     TRUE           1
## 23  llegada_random6   6.272161 11.272161             5     TRUE           1
## 24    llegada_from4   6.496737 11.496737             5     TRUE           1
## 25  llegada_random7   6.617279 11.617279             5     TRUE           1
## 26  llegada_random8   6.692181 11.692181             5     TRUE           1
## 27 llegada_from_to5   6.993783 11.993783             5     TRUE           1
## 28 llegada_from_to6   7.467738 12.467738             5     TRUE           1
## 29  llegada_random9   7.879289 12.879289             5     TRUE           1
## 30    llegada_from5   8.315619 13.315619             5     TRUE           1
## 31 llegada_from_to7   9.000000 14.000000             5     TRUE           1
## 32 llegada_from_to8   9.440001 14.440001             5     TRUE           1
## 33 llegada_random10   9.517035 14.517035             5     TRUE           1
## 34    llegada_from6   9.715277 14.715277             5     TRUE           1
## 35 llegada_random11   9.951204 14.951204             5     TRUE           1
```

También podemos desencadenar llegadas a demanda desde otra trayectoria.
Sería el ejemplo de un operador, que en el momento en que ficha digitalmente como trabajador de la empresa, se incorpora a trabajar durante una jornada parcial de 3 horas:


```r
# jornada de trabajo
t0 <- trajectory() %>%
  timeout(3)
# activa "Trabajar": la jornada de trabajo
t1 <- trajectory() %>%
  activate("Trabajar")

simmer() %>%
  # simula llegadas al trabajo cuando "Trabajar" esté activado
  add_generator("Trabajar", t0, when_activated()) %>%
  # simula una llegada a fichar
  add_generator("Fichado digital", t1, at(8)) %>%
  run() %>%
  get_mon_arrivals()
```

```
##               name start_time end_time activity_time finished replication
## 1 Fichado digital0          8        8             0     TRUE           1
## 2        Trabajar0          8       11             3     TRUE           1
```

La sintaxis que se utiliza para generar llegadas es:

    add_generator(.env, name_prefix, trajectory, distribution, mon = 1,
      priority = 0, preemptible = priority, restart = FALSE)

El argumento `mon` permite especificar si queremos que el simulador monitoree las llegadas: 0 = sin monitoreo, 1 = monitoreo simple de llegadas simple, 2 = monitoreo simple y de atributos de las llegadas.
Por defecto tendremos `mon=1`.
En el argumento `distribution` se pueden utilizar funciones definidas por el usuario para los tiempos entre llegadas y también, como hemos visto, `at()`, `from()`, `to()` y `from_to()`.
Si hay un sistema de prioridades, se especificaría con `priority=1`, y qué hacer para las llegadas preferentes o prioritarias, en los argumentos `preemptible` y `restart`.

-   **Fuentes de datos** con tiempos entre llegadas, provenientes de un data frame que vincula el usuario con el comando [`add_dataframe()`](https://r-simmer.org/reference/add_dataframe.html):

```{=html}
<!-- -->
```
    add_dataframe(.env, name_prefix, trajectory, data, mon = 1, batch = 50,
      col_time = "time", time = c("interarrival", "absolute"),
      col_attributes = NULL, col_priority = "priority",
      col_preemptible = col_priority, col_restart = "restart")

Ambos generadores o fuentes de llegadas se reprograman a sí mismos para ciclarse hasta agotar el tiempo de simulación, en el caso de que este supere el rango inicial.

## Recursos

Recordemos que un recurso, que proporciona servicio a las llegadas, en la simulación DES tiene dos componentes internos: un servidor y una cola.
Todo recurso va a ser definido por tres parámetros:

-   el nombre (*name*) del recurso o servidor,
-   la capacidad (*capacity*) del recurso, y
-   el tamaño de la cola (*queue_size*) en la que esperan las llegadas a que haya algún recurso disponible; representa el aforo de la sala de espera; cuando es cero significa que no hay posibilidad de cola.

Los recursos se definen con la sintaxis:

    add_resource(.env, name, capacity = 1, queue_size = Inf, mon = TRUE,
      preemptive = FALSE, preempt_order = c("fifo", "lifo"),
      queue_size_strict = FALSE, queue_priority = c(0, Inf))

El argumento `capacity` indica cuántos servidores hay en el sistema, y `queue_size` el tamaño de la cola de espera.
Por defecto, los recursos son monitorizados (`mon=TRUE`) y no preferentes (`preemptive = FALSE`).
La preferencia significa que si aparece una llegada con prioridad alta, el recurso detendrá temporalmente el procesado de las llegadas que esté atendiendo y tengan menos prioridad, y atenderá la preferente.
En los recursos preferentes, `preemptive = TRUE` el parámetro `preempt_order` define qué llegada debe priorizarse según una política FIFO (*First in, first out*)o LIFO (*Last in, first out*).
Todas las llegadas con mayor preferencia son ubicadas en una cola especial que tiene mayor prioridad que la cola principal, y en consecuencia se atienden antes.
Además, el parámetro `queue_size_strict` controla si esta cola especial de llegadas preferentes debe ser tenida en cuenta para calcular el tamaño límite de la cola; si este parámetro impone el límite, entonces los rechazos se producirán en la cola principal.

Para ampliar información, consultar el Manual de Referencia sobre [`add_resource`](https://r-simmer.org/reference/add_resource.html).

Cuando tenemos recursos en el sistema que dan un servicio, surgen dos acciones básicas que son: `seize` o asignación a un recurso, y `release` o desocupación del recurso.
Por supuesto, si una llegada no puede ser atendida por un servidor (recurso) libre, puede esperar en cola si hay hueco en la cola, o ser rechazada si no lo hay.

Transformemos el ejemplo anterior en clientes que llegan aleatoriamente a una tienda.
Ahora al llegar, cada cliente mira productos alrededor de 5 minutos, luego busca a un dependiente para ser atendido o espera hasta que uno esté desocupado, este lo atiende por aproximadamente 10 minutos y luego se va.
Supongamos que hay 2 dependientes en la tienda.


```r
set.seed(999)
env=simmer()

# Actividades que se desarrollan en la tienda
tienda=trajectory() %>%
  log_(function()  "Llega a la tienda") %>%
  # mira productos aprox. 5min
  timeout(function() rnorm(1,5,1)) %>% 
  # es asignado a un dependiente 
  log_(function() "Busca un dependiente") %>%
  seize("dependiente",1) %>%
  # que lo antiende aprox. 10min.
  log_("Es atendido") %>%
  timeout(function() rnorm(1,10,1)) %>%
  # desocupa al dependiente
  release("dependiente",1)%>%
  # sale de la tienda
  log_(function() "Sale de la tienda")

env=env %>%
  # simulación (uniforme(0,5) del tiempo entre llegadas de clientes
  add_generator("cliente", tienda,function() runif(1,0,5)) %>% 
  # dimensionamiento de recursos: dos dependientes
  add_resource("dependiente",2) %>%  
  print() %>%
  run(20) %>% # simulamos hasta ese instante
  # monitorizamos los recursos (dependientes)
  get_mon_resources()
```

```
## simmer environment: anonymous | now: 0 | next: 0
## { Monitor: in memory }
## { Resource: dependiente | monitored: TRUE | server status: 0(2) | queue status: 0(Inf) }
## { Source: cliente | monitored: 1 | n_generated: 0 }
## 1.94536: cliente0: Llega a la tienda
## 4.86066: cliente1: Llega a la tienda
## 5.6328: cliente0: Busca un dependiente
## 5.6328: cliente0: Es atendido
## 8.68238: cliente1: Busca un dependiente
## 8.68238: cliente1: Es atendido
## 8.79439: cliente2: Llega a la tienda
## 11.55: cliente3: Llega a la tienda
## 11.9157: cliente2: Busca un dependiente
## 12.0631: cliente4: Llega a la tienda
## 14.2341: cliente0: Sale de la tienda
## 14.2341: cliente2: Es atendido
## 15.9421: cliente4: Busca un dependiente
## 16.1694: cliente5: Llega a la tienda
## 17.3468: cliente3: Busca un dependiente
## 18.9358: cliente6: Llega a la tienda
## 18.9865: cliente1: Sale de la tienda
## 18.9865: cliente4: Es atendido
```

Visualizamos a los clientes: cuándo llegan, cuándo son atendidos por un dependiente y cuándo se van de la tienda.
Para los dependientes (recursos) vemos los instantes de tiempo (time) en los que inician un servicio o se les añade algún cliente en cola, el número de dependientes ocupados (server) y el número de clientes esperando ser atendidos (queue).

## Acciones {#acciones}

Las acciones posibles para definir trayectorias las presentamos a continuación, categorizadas según su funcionalidad:

+-----------------------------------+-----------------------------------------------------------------------------------------+
| Funciones                         | comandos                                                                                |
+===================================+=========================================================================================+
| atributos en llegadas             | set_attribute, get_attribute, set_prioritization, get_prioritization                    |
+-----------------------------------+-----------------------------------------------------------------------------------------+
| interacción con recursos          | seize, release, select, set_capacity, set_queue_size,                                   |
+-----------------------------------+-----------------------------------------------------------------------------------------+
| interacción con fuentes           | activate, deactivate, set_trajectory, set_source                                        |
+-----------------------------------+-----------------------------------------------------------------------------------------+
| ramificación                      | branch                                                                                  |
+-----------------------------------+-----------------------------------------------------------------------------------------+
| retrasos                          | timeout, timeout_from_attribute, time_out_fromglobal                                    |
+-----------------------------------+-----------------------------------------------------------------------------------------+
| clonación y sincronización        | clone, synchronize                                                                      |
+-----------------------------------+-----------------------------------------------------------------------------------------+
| bucles                            | rollback                                                                                |
+-----------------------------------+-----------------------------------------------------------------------------------------+
| ejecución en lotes                | batch, separate                                                                         |
+-----------------------------------+-----------------------------------------------------------------------------------------+
| programación asíncrona            | send, wait, trap, untrap                                                                |
+-----------------------------------+-----------------------------------------------------------------------------------------+
| renuncias                         | leave, renege_in, renege_if, renege_abort                                               |
+-----------------------------------+-----------------------------------------------------------------------------------------+
| comunicación entre llegadas       | send, trap, untrap, wait                                             |
+-----------------------------------+-----------------------------------------------------------------------------------------+
| depuración                        | log_, stop_if                                            |
+-----------------------------------+-----------------------------------------------------------------------------------------+

La Figura 3 muestra con mucha claridad las principales acciones disponibles en `simmer`.

\begin{figure}

{\centering \includegraphics[width=1\linewidth]{images/simmer_trajectory} 

}

\caption{Figura 3. Principales acciones en trayectorias `simmer`. [Fte: IBiDat](https://ibidat.es/portfolio-items/simmer-discrete-event-simulation-for-r/)}(\#fig:simmertray)
\end{figure}

## Atributos

Las llegadas pueden almacenar y modificar características específicas o atributos con el comando `set_attribute()`.
Los atributos consisten en pares `(key,value)` que por defecto se asignan por llegada, salvo que se definan globalmente con `set_global()`.
Además, pueden ser vectores o funciones que devuelvan vectores.
Sirven por ejemplo si queremos asignar costes a cada llegada, o identificar la ruta que siguen si hay varias posibles, etc.

La especificación de atributos es una acción más que se desarrolla dentro de una trayectoria a las llegadas que son dirigidas a ella.
Se especifican con el comando [`set_attribute()`](https://r-simmer.org/reference/set_attribute.html):

    set_attribute(keys, values, mod = c(NA, "+", "*"), init = 0)

donde 'key' es el nombre del atributo o característica que asignamos a las llegadas, y'value' es el valor numérico que se le asigna (cuando queramos asignar atributos textuales habremos de codificarlos a numéricos).
Con 'mod' opcionalmente se pueden modificar los atributos (por ejemplo un contador que suma uno en cada llegada utilizaría mod="+"), que se inicializaría en el valor eespecificado en 'ini'.

Para visualizar el valor de un atributo que previamente hemos asignado podemos usar el comando [`get_attribute()`](https://r-simmer.org/reference/get_n_generated.html).
Cuando lanzamos un proceso, si queremos visualizar los atributos asignados a cada una de las llegadas, utilizaremos el comando [`get_mon_attributes()`](https://r-simmer.org/reference/get_mon.html) asegurándonos antes de que hemos incluido en las condiciones de monitorización de las llegadas a las que les hemos asignado esos atributos, la opción 'mon=2' (en `add_generator()`).

Por ejemplo, el siguiente modelo define la llegada de un sujeto al que se le da el atributo 'peso=60', lo divulga, y tras comer sube un kilo y vuelve a comunicarlo.


```r
env=simmer()
traj=trajectory() %>%
  # asignamos el atributo numérico a peso
  set_attribute("peso",60) %>%
  log_(function() paste("Hoy peso",get_attribute(env,"peso"), "kg. Me voy a comer.")) %>%
  # a continuación lo modificamos sumándole 1
  set_attribute("peso",1,mod="+") %>%
  # y por último lo visualizamos
  log_(function() paste("Mi peso ahora es",get_attribute(env,"peso")))

env %>%
  add_generator("peso",traj,at(1),mon=2) %>%
  run() %>%
  get_mon_attributes()
```

```
## 1: peso0: Hoy peso 60 kg. Me voy a comer.
## 1: peso0: Mi peso ahora es 61
```

```
##   time  name  key value replication
## 1    1 peso0 peso    60           1
## 2    1 peso0 peso    61           1
```

Las llegadas además, están sujetas a tres posibles valores de prioridad para acceder a los recursos:

-   **prioridad** (*priority*).
    Se aplica a las llegadas.
    Un valor más alto implica mayor prioridad.
    El valor por defecto que se asigna a todas las llegadas es 0, esto es, la mínima prioridad posible.

-   **preferente** (*preemptible*).
    Se aplica a los recursos y establece la mínima prioridad con que se accede a dicho recurso.
    Si un recurso tiene cierto valor de preferencia y aparece una llegada con prioridad mayor a ese valor de preferencia, esta llegada se antepone en cola a cualquier otra que tenga prioridad inferior; las actividades en curso no se interrumpen. Sólo las llegadas con prioridad más alta pueden desencadenar una preferencia en el servicio o suspensión inesperada por tener llegadas con mayor prioridad.

-   **reinicio** (*restart*), cuando se ha producido un servicio con preferencia, indica si al finalizar la actividad con la llegada prioritaria, el recurso ha de reanudar (TRUE) la actividad previa que hubo de suspender o no (0).

Estas características se definen con el comando [`set_prioritization()`](https://r-simmer.org/reference/set_prioritization.html), y se recuperan con el comando [`get_prioritization()`](https://r-simmer.org/reference/get_n_generated.html).

    set_prioritization(.trj, values, mod = c(NA, "+", "*"))

El argumento `values=c(priority,preemptible,restart)` espera un vector numérico con tres elementos que representen la prioridad, preferencia y reinicio que han de aplicarse en una trayectoria dada antes de acceder a algún recurso.
El argumento `mod` permite de nuevo modificar automáticamente estos valores en cada llegada.

Simulemos un proceso en el que llegan pacientes al triaje de un servicio de urgencias de un hospital en diferentes instantes de tiempo, que resultan ser graves o no con cierta probabilidad. Cuando llega un paciente grave (prioridad=1), es derivado inmediatamente a un médico y accede al servicio de atención tan pronto acabe la atención al paciente que está atendiendo en ese momento. Tras atenderlo, el médico vuelve a atender al resto de pacientes por orden de prioridad.

Especificamos en este caso, para cada llegada, una simulación $Ber(p)$ que establece si el paciente está grave (valor 1). La preferencia del recurso se fija también a 1, para garantizar que dará preferencia a todas las llegadas con prioridad mayor o igual a 1. No se da valor a `restart`, asumiendo que por defecto continuará con los pacientes en el orden de llegada-prioridad.


```r
set.seed(1)
env=simmer()

triaje = trajectory() %>%
  set_prioritization(function() c(rbinom(1,1,0.4),1,NA)) %>%
  # guardamos como atributo la prioridad asignada
  set_attribute("Prioridad",function() get_prioritization(env)[1]) %>% 
  # se redirige al médico
  seize("medico",1) %>%
  # que tarda cierto tiempo en atenderlo
  timeout(function() rexp(1,0.2)) %>%
  release("medico",1)
  

env %>% 
  add_generator("paciente",triaje,at(0:4),mon=2) %>%
  add_resource("medico",1) %>%
  run() 
```

```
## simmer environment: anonymous | now: 31.4096241788118 | next: 
## { Monitor: in memory }
## { Resource: medico | monitored: TRUE | server status: 0(1) | queue status: 0(Inf) }
## { Source: paciente | monitored: 2 | n_generated: 5 }
```

```r
#visualizamos el output
merge(get_mon_arrivals(env),get_mon_attributes(env))
```

```
##        name replication start_time  end_time activity_time finished time       key value
## 1 paciente0           1          0  5.908214      5.908214     TRUE    0 Prioridad     0
## 2 paciente1           1          1 28.711210      6.147810     TRUE    1 Prioridad     0
## 3 paciente2           1          2  8.088557      2.180343     TRUE    2 Prioridad     1
## 4 paciente3           1          3 31.409624      2.698414     TRUE    3 Prioridad     0
## 5 paciente4           1          4 22.563400     14.474843     TRUE    4 Prioridad     1
```

## Interacción con recursos

Las dos actividades principales para interaccionar con recursos son `seize()` -acceso- y `release()` -salida-.
Un recurso o conjunto de recursos (*amount*) entran en funcionamiento con `seize()` y dejan de funcionar con `release()`.

    seize(.trj, resource, amount = 1, continue = NULL, post.seize = NULL,reject = NULL)
    release(.trj, resource, amount = 1)

La actividad `seize()` es especial en el sentido de que el resultado o salida depende del estado del recurso.
Una llegada, cuando accede a un recurso, puede aprovecharlo con éxito desde el momento en que accede y continuar su camino en la trayectoria, pero también puede ponerse en cola (si el recurso está ocupado) o ser rechazada y despedida de la trayectoria.
Para manejar estos casos especiales con total flexibilidad, `seize()` soporta la especificación de dos sub-trayectorias opcionales:

-   `post.seize`, que es la actividad que sigue despues de un *seize* exitoso, y
-   `reject`, que es la actividad que sigue si la llegada ha sido rechazada.

Como en todas las actividades que soportan la definición de sub-trayectorias, hay un parámetro booleano llamado *continue* (=TRUE o FALSE). Para cada sub-trayectoria, este parámetro controla si las llegadas deberían continuar a la actividad que sigue a `seize()` en la trayectoria principal después de ejecutar la sub-trayectoria.

Para ejemplificar su uso consideramos un ambulatorio médico al que llegan pacientes.
Si al llegar el paciente, el doctor está disponible, es atendido por este durante 5 minutos.
Si no está disponible, es rechazado y derivado a consulta de enfermería, donde es atendido siempre 8 minutos, al haber 10 enfermeras disponibles.
En ambos casos el paciente se va del ambulatorio al terminar la consulta a la que haya entrado.


```r
env=simmer()

traj=trajectory() %>%
  log_(function() paste("Llega el ",get_name(env))) %>%
  seize(
    "doctor",1,continue=c(TRUE,FALSE),
    post.seize=trajectory("Paciente aceptado") %>% 
      log_("Pasa a consulta con el doctor"), 
    reject=trajectory("Paciente rechazado") %>% 
      log_("El doctor está ocupado y es derivado a enfermería") %>% 
      seize("nurse",1) %>%
      log_("Pasa a consulta con la enfermera") %>%
      timeout(8) %>%
      release("nurse",1) %>%
      log_(function() paste("El",get_name(env), "sale de enfermería"))) %>%
  timeout(5) %>% 
  release("doctor",1) %>% 
  log_("El doctor ya está libre")

env=env %>%
   add_resource("doctor",capacity=1,queue_size=0) %>% 
   add_resource("nurse",capacity=10,queue_size=0) %>%
   add_generator("paciente",traj,at(0,1,5,8)) %>%
   run()
```

```
## 0: paciente0: Llega el  paciente0
## 0: paciente0: Pasa a consulta con el doctor
## 1: paciente1: Llega el  paciente1
## 1: paciente1: El doctor está ocupado y es derivado a enfermería
## 1: paciente1: Pasa a consulta con la enfermera
## 5: paciente0: El doctor ya está libre
## 5: paciente2: Llega el  paciente2
## 5: paciente2: Pasa a consulta con el doctor
## 8: paciente3: Llega el  paciente3
## 8: paciente3: El doctor está ocupado y es derivado a enfermería
## 8: paciente3: Pasa a consulta con la enfermera
## 9: paciente1: El paciente1 sale de enfermería
## 10: paciente2: El doctor ya está libre
## 16: paciente3: El paciente3 sale de enfermería
```

A la hora de asignar un recurso, podemos hacerlo especificando explícitamente el nombre del recurso (si tenemos varios), o hacerlo de forma dinámica especificando la política a seguir.
Esto lo podemos hacer con la actividad `select()`, a través de los argumentos *resources* y *policy*.

    select(.trj, resources, policy = c("shortest-queue",
      "shortest-queue-available", "round-robin", "round-robin-available",
      "first-available", "random", "random-available"), id = 0)

Hay varias políticas implementadas internamente a las que se puede acceder por su nombre:

-   'shortest-queue': se selecciona el recurso con la cola más corta
-   'round-robin': se seleccionarán los recursos de una forma cíclica
-   'first-available': se selecciona el recurso que queda disponible el primero
-   'random': se selecciona un recurso aleatoriamente.

El parámetro *resources* también puede ser dinámico e incluso existe la posibilidad de definir políticas específicas.
Una vez que un recurso es seleccionado, hay versiones especiales de las actividades mencionadas para interaccionar con los recursos sin especificar su nombre, como son `seize_selected()`, `set_capacity_selected()`, etc.

Pongamos como ejemplo una consulta médica con 3 doctores, que atienden a los pacientes de un modo ordenado secuencial conforme llegan a la consulta.


```r
traj <- trajectory() %>%
  simmer::select(paste0("doctor", 1:3), "round-robin") %>%
  seize_selected(1) %>%
  timeout(5) %>%
  release_selected(1)

simmer() %>%
  add_resource("doctor1") %>%
  add_resource("doctor2") %>%
  add_resource("doctor3") %>%
  add_generator("patient", traj, at(0, 1, 2)) %>%
  run() %>%
  get_mon_resources()
```

```
##   resource time server queue capacity queue_size system limit replication
## 1  doctor1    0      1     0        1        Inf      1   Inf           1
## 2  doctor2    1      1     0        1        Inf      1   Inf           1
## 3  doctor3    2      1     0        1        Inf      1   Inf           1
## 4  doctor1    5      0     0        1        Inf      0   Inf           1
## 5  doctor2    6      0     0        1        Inf      0   Inf           1
## 6  doctor3    7      0     0        1        Inf      0   Inf           1
```

## Interacción con fuentes

Hay cuatro actividades específicas para modificar las fuentes de llegadas.
Una llegada puede activar `activate()` o desactivar `deactivate()` una fuente, pero también puede modificar la trayectoria a la que se adhieren las llegadas que aparecen (derivarlas a otras trayectorias), con `set_trajectory()`, o especificar una nueva distribución entre-llegadas con `set_source()`.
Para seleccionar dinámicamente una fuente, el parámetro que especifica el nombre de la fuente en todos estos métodos puede ser dinámico.

    activate(.trj, sources)
    deactivate(.trj, sources)
    set_trajectory(.trj, sources, trajectory)
    set_source(.trj, sources, object)  

En el ejemplo a continuación, una llegada, al acceder a la trayectoria, desactiva la fuente que genera llegadas cada segundo, deja transcurrir 1 segundo y después vuelve a activarla.


```r
traj=trajectory() %>%
  deactivate("dummy") %>% 
  timeout(1) %>% 
  activate("dummy") 

simmer() %>%
  add_generator("dummy",traj,function() 1) %>% 
  run(10) %>% 
  get_mon_arrivals()
```

```
##     name start_time end_time activity_time finished replication
## 1 dummy0          1        2             1     TRUE           1
## 2 dummy1          3        4             1     TRUE           1
## 3 dummy2          5        6             1     TRUE           1
## 4 dummy3          7        8             1     TRUE           1
```

En este otro ejemplo se define un simulador que genera llamadas cada 2 segundos y las redirige a la trayectoria 'traj2.' Una vez allí, la fuente de llamadas se modifica para que a partir de que la primera llamada finalice (estando 2 segundos en 'traj2'), se generen llamadas cada segundo y se redirijan estas a la trayectoria 'traj1'.


```r
traj1 <- trajectory() %>%
  timeout(1)

traj2 <- trajectory() %>%
  set_source("llamada", function() 1) %>%
  set_trajectory("llamada", traj1) %>%
  timeout(2)

simmer() %>%
  add_generator("llamada", traj2, function() 2) %>%
  run(6) %>%
  get_mon_arrivals()
```

```
##       name start_time end_time activity_time finished replication
## 1 llamada0          2        4             2     TRUE           1
## 2 llamada1          3        4             1     TRUE           1
## 3 llamada2          4        5             1     TRUE           1
```

## Ramificación

Una rama (branch) es un punto en una trayectoria en el cual se pueden seguir una o más sub-trayectorias.
`simmer` soporta dos tipos de ramificación:

-   La actividad `branch()` coloca la llegada en una de las sub-trayectorias que dependen de alguna condición evaluada en un parámetro dinámico llamado *option*.
    Es el equivalente de una condición *if/else*, es decir, si el valor de *option* es 'i', entonces se ejecutará la sub-trayectoria 'i'.

-   Por otro lado, la actividad `clone()` genera n ramas paralelas (clonadas) y replica la llegada n-1 veces, colocando cada una de ellas en las n sub-trayectorias creadas.
    `clone()` es la única actividad de sub-trayectorias que no acepta un parámetro *continue*.
    Por defecto todos los clones continúan en la trayectoria principal después de esta actividad.
    Para borrar todos los clones excepto uno, se utiliza la actividad `synchronize()`.

```{=html}
<!-- -->
```
    branch(.trj, option, continue, ...)
    clone(.trj, n, ...)
    synchronize(.trj, wait = TRUE, mon_all = FALSE)

En el ejemplo a continuación simulamos un juego en el que el jugador lanza una moneda.
Si llega durante la primera hora (*now(env)=1*), gana un caramelo y se va, y si no, pierde dos caramelos que ha de regalar a María y a José.


```r
env=simmer()

traj=trajectory() %>%
  branch(
    option=function() now(env), continue=c(FALSE,TRUE), #si 1 la 1ª trayectoria y se va; si 2 la 2ª trayectoria y sigue
      trajectory() %>% log_(function() paste(get_name(env),"Ha llegado el instante",now(env),", gana un caramelo y se va.")), 
      trajectory() %>% log_(function() paste(get_name(env),"Ha llegado el instante",now(env),"y regala dos caramelos"))) %>% 
  clone(n=2, 
    trajectory() %>% log_("uno a María"), 
    trajectory() %>% log_("otro a José")) %>%  
  synchronize() 
  
env %>%
  add_generator("Jugador",traj,at(1,2)) %>%
  run() %>%
  invisible
```

```
## 1: Jugador0: Jugador0 Ha llegado el instante 1 , gana un caramelo y se va.
## 2: Jugador1: Jugador1 Ha llegado el instante 2 y regala dos caramelos
## 2: Jugador1: uno a María
## 2: Jugador1: otro a José
```

## Bucles

Hay un comando llamado `rollback()` útil para moverse hacia atrás en una trayectoria y ejecutar bucles sobre una serie de actividades.
Esta actividad provoca que la llegada retroceda un cierto número de actividades *amount* (que pueden ser dinámicas) un cierto número de veces *times*.
Si se utiliza una función de chequeo *check*, el parámetro *times* es ignorado y la llegada determina si debe retroceder cada vez que choca con *rollback*.

    rollback(.trj, amount, times = Inf, check = NULL)

En el siguiente ejemplo, un jugador accede a un reto de resistencia en el que ha de levantar pesas y mantenerlas.
Por cada 5 minutos que aguante, recibirá una recompensa de 25€.
El jugador es capaz de aguantar 30 minutos, pero la máxima recompensa a recibir es de 100€, por lo que se detendrá cuando consiga a esa cantidad.


```r
env <- simmer()

traj <- trajectory() %>%
  set_attribute("var", 0) %>%
  log_(function()
    paste("Tiempo de resistencia:",now(env),". Ganancia=", get_attribute(env, "var"))) %>%
  set_attribute("var", 25, mod="+") %>%
  timeout(5)%>%
  rollback(3, check=function() get_attribute(env, "var") <= 100) 

env %>%
  add_generator("dummy", traj, at(0)) %>%
  run() %>% invisible
```

```
## 0: dummy0: Tiempo de resistencia: 0 . Ganancia= 0
## 5: dummy0: Tiempo de resistencia: 5 . Ganancia= 25
## 10: dummy0: Tiempo de resistencia: 10 . Ganancia= 50
## 15: dummy0: Tiempo de resistencia: 15 . Ganancia= 75
## 20: dummy0: Tiempo de resistencia: 20 . Ganancia= 100
```

## Ejecución en lotes

La ejecución en lotes o *batching* consiste en acumular varias llegadas antes de que puedan continuar su camino en la trayectoria de modo unitario.
Esto significa, por ejemplo, que si 10 llegadas en un lote intenta acceder a una unidad de cierto recurso, sólo una unidad puede ser asignada, y no las 10.
Un lote se puede descomponer con `separate()`, salvo que se marque como `permanent=TRUE`.

Por defecto todas las llegadas que acceden a un lote se juntan en él, y esperan hasta que se consigue el número esperado de llegadas *n*.
Sin embargo, las llegadas pueden evitar unirse al lote bajo alguna restricción si se proporciona una función booleana opcional, *rule*.
También un lote se puede agrupar para acceder a un recurso antes de acumular el tamaño del lote, si se especifica un tiempo límite para acceder con *timeout*.
Los lotes se comparten sólo por llegadas que se añaden a la misma trayectoria directamente.
Siempre que se necesite un lote compartido globalmente, se ha de especificar un nombre con *name*.

    batch(.trj, n, timeout = 0, permanent = FALSE, name = "", rule = NULL)
    separate(.trj)

Un ejemplo de sistema en el que puede tener sentido la ejecución por lotes es la visita guiada a un museo, en la que son precisos 5 visitantes para asignar un guía e iniciar la visita.
Si en 10 minutos no se han conseguido los visitantes, se iniciará la visita igualmente con los visitantes disponibles.
La visita dura 5 minutos y, puesto que hay un único guía, será preciso esperar a su finalización para empezar la siguiente.
Los tiempos entre llegadas provienen de una distribución exponencial de media 5.


```r
set.seed(1234)
env=simmer()

visita=trajectory() %>%
  batch(n=5,timeout=10,name="visitaguiada",permanent=FALSE) %>%
  seize("guia",1) %>%
  log_("Comienza la visita con el guía") %>%
  timeout(5) %>%
  release("guia",1) %>%
  log_("Visita terminada") 
  

env=env %>%
  add_resource("guia",1) %>%
  add_generator("visitante", visita,function() rnorm(1,5,0.5)) %>%
  print() %>%
  run(until=50) 
```

```
## simmer environment: anonymous | now: 0 | next: 0
## { Monitor: in memory }
## { Resource: guia | monitored: TRUE | server status: 0(1) | queue status: 0(Inf) }
## { Source: visitante | monitored: 1 | n_generated: 0 }
## 14.3965: batch_visitaguiada: Comienza la visita con el guía
## 19.3965: batch_visitaguiada: Visita terminada
## 25.0774: batch_visitaguiada: Comienza la visita con el guía
## 30.0774: batch_visitaguiada: Visita terminada
## 39.3721: batch_visitaguiada: Comienza la visita con el guía
## 44.3721: batch_visitaguiada: Visita terminada
```

```r
get_mon_arrivals(env)
```

```
##         name start_time end_time activity_time finished replication
## 1 visitante0   4.396467 19.39647             5     TRUE           1
## 2 visitante1   9.535182 19.39647             5     TRUE           1
## 3 visitante2  15.077402 30.07740             5     TRUE           1
## 4 visitante3  18.904553 30.07740             5     TRUE           1
## 5 visitante4  24.119116 30.07740             5     TRUE           1
## 6 visitante5  29.372144 44.37214             5     TRUE           1
## 7 visitante6  34.084774 44.37214             5     TRUE           1
## 8 visitante7  38.811458 44.37214             5     TRUE           1
```

## Programación asíncrona

Hay ciertos métodos que permiten eventos asíncronos:

-   La actividad `send()` emite una o más señales (*signals*) a todas las llegadas que se suscriben a ellas.
    Las señales se pueden desencadenar inmediatamente o después de cierto retraso (*delay*).
    En ese caso, los dos parámetros, *signals* y *delay*, pueden ser dinámicos.

-   Las llegadas pueden bloquearse y esperar con `wait()` hasta que se recibe cierta señal.

-   Con `trap()` las llegadas pueden suscribirse a señales (*signals*) y (opcionalmente) asignar un manipulador (de señales) *handler*.
    Si se proporciona el *handler*, la llegada detiene la atividad actual hasta que recibe la señal.
    Después, la actividad continúa desde el punto de interrupción.
    Sin embargo, si la llegada está esperando en un recurso de cola, las señales atrapadas se ignoran.
    Lo mismo ocurre con un lote: todas las señales suscritas antes de entrar a un lote son ignoradas.

-   Finalmente la actividad `untrap()`se puede utilizar para cancelar la suscripción a *signals*.

```{=html}
<!-- -->
```
    send(.trj, signals, delay = 0)
    wait(.trj)
    trap(.trj, signals, handler = NULL, interruptible = TRUE)
    untrap(.trj, signals)

Por defecto los manipuladores de señales (*signal handlers*) pueden ser interrumpidos también por otras señales, lo que significa que un manipulador (*handler*) puede permanecer reiniciándose si se están emitiendo suficientes señales.
Si se necesita un manipulador que no pueda ser interrumpido, hay que utilizar el parámetro `interruptible=FALSE` en `trap()`.

En el ejemplo a continuación, una inteligencia artificial reclama una clave de acceso al usuario para desbloquear un recurso digital y permitirle el acceso.
El usuario proporciona la clave de acceso al cabo de 5 segundos.
La IA recibe la clave y desbloquea el recurso.


```r
ia.acceso=trajectory() %>%
  log_("Solicito clave de acceso") %>%
  trap("clave") %>%
  wait() %>%  
  log_("Clave recibida y acceso permitido")

usuario=trajectory() %>%
  log_("Clave enviada") %>%
  send("clave") 
  
simmer() %>%
  add_generator("ia.acceso",ia.acceso,at(0)) %>%
  add_generator("usuario",usuario,at(5)) %>%
  run() %>%
  invisible
```

```
## 0: ia.acceso0: Solicito clave de acceso
## 5: usuario0: Clave enviada
## 5: ia.acceso0: Clave recibida y acceso permitido
```

## Renuncias

Además de ser rechazadas cuando tratan de acceder a un recurso, las llegadas pueden dejar una trayectoria en cualquier momento, síncrona o asíncronamente.
En principio, renunciar significa que una llegada abandona la trayectoria en un momento dado.
La actividad más simple que permite esto es `leave()`, que inmediatamente desencadena la acción dada alguna probabilidad.
Además, `renege_in()` y `renege_if()` desencadena la renuncia asíncronamente después de algún tiempo *timeout=t* o si se recibe una señal, respectivamente, salvo que la acción sea abortada con `renege_abort()`.
Los dos métodos `renege_in()` y `renege_if()` aceptan una subtrayectoria opcional, *out*, que se ejecuta directamente antes de abandonar la trayectoria.

    leave(.trj, prob, out = NULL, keep_seized = TRUE)
    renege_in(.trj, t, out = NULL, keep_seized = FALSE)
    renege_if(.trj, signal, out = NULL, keep_seized = FALSE)
    renege_abort(.trj)

En el ejemplo siguiente los clientes llegan a un banco con un solo empleado, del que se


```r
banco=trajectory() %>%
  log_("He llegado al banco") %>%
  # se marcha si no es atendido en 5 minutos
  renege_in(
    5,
    out=trajectory() %>%
      log_("He colmado mi paciencia. Me voy...")) %>%
  seize("empleado",1) %>%
  # me quedo si soy atendido en 5 minutos
  renege_abort() %>%
  log_("Me están atendiendo") %>%
  timeout(10) %>%
  release("empleado",1) %>%
  log_("He terminado")

simmer() %>%
  add_resource("empleado",1) %>%
  add_generator("cliente",banco,at(0,1)) %>%
  run() %>%
  invisible
```

```
## 0: cliente0: He llegado al banco
## 0: cliente0: Me están atendiendo
## 1: cliente1: He llegado al banco
## 6: cliente1: He colmado mi paciencia. Me voy...
## 10: cliente0: He terminado
```

## Ejemplos

El concepto de trayectoria desarrollado en `simmer` da lugar a un modo natural de simular una amplia variedad de problemas relacionados con Cadenas de Markov continuas en el tiempo (CTMC), procesos de nacimiento-muerte y sistemas de colas.

### Proceso industrial

Utilizamos la ilustración de un proceso industrial de Pidd (1988), Section 5.3.1, base de la introducción del artículo de Ucar et al (2019), en el que describe de modo completo la simulación del proceso con `simmer`, y que traducimos y comentamos a continuación.

Consideramos un negocio de trabajos de ingeniería, en la que hay varias máquinas idénticas.
Cada máquina es capaz de procesar cualquier trabajo que llegue.
Hay un suministro de trabajos sin perspectivas de escasez.
Los trabajos se asignan a la primera máquina disponible.
El tiempo en completar un trabajo es variable, pero depende de la máquina que se utilice.
Las máquinas están a cargo de los operarios, que las revisa y realizan una serie de tareas sobre ellas.

El proceso está pues constituido por dos tipos de recursos:

-   máquinas, a las que llegan trabajos (jobs)
-   operarios, a los que llegan tareas (tasks)

Respecto a cómo ocupan su tiempo de operatividad, distinguimos entre **estados** y **actividades**.
Estas últimas se refieren a tareas que conllevan cierto tiempo para ser completadas.
En la Figura

Las actividades que realizan los operarios son:

-   RESET: resetear/reinicializar la máquina, si no muestra desgaste que afecte a su funcionalidad
-   RETOOL: reparar la máguina si aprecia algún desgaste que afecta al funcionamiento. Después de una reparación (RETOOL) y antes de reiniciar el trabajo, toda máquina ha de ser reinicializada (RESET).
-   AWAY: salir un rato para atender sus necesidades personales.

La actividad que llevan a cabo las máquinas es la propia realización del trabajo (RUNNING).

Se pueden identificar dos clases de procesos o fuentes de llegadas, que generan:

-   los *trabajos de tienda* (shop jobs), que usan las máquinas y las desgastan,
-   las *tareas personales* que realizan los operarios que se ausentan (AWAY).

En la Figura 1 está representado el flujo de trabajo de este sistema basado en máquinas (a la izquierda) y operarios (a la derecha).
En círculos los estados posibles de los recursos y en rectángulos las actividades a completar.

\begin{figure}

{\centering \includegraphics[width=0.8\linewidth]{images/job-cycle} 

}

\caption{Figura 1. Ciclo de trabajo del sistema descrito en Pidd (1988)}(\#fig:job-cycle)
\end{figure}

La forma de simular este sistema con `simmer` consiste en considerar las máquinas y operarios como recursos y describir como trayectorias los ciclos de los trabajos de tienda y de las tareas personales.

En primer lugar inicializamos un nuevo entorno de simulación y definimos el tiempo de compleción de las diferentes actividades, que vamos a definir según distribuciones exponenciales.
Asímismo, se definen los tiempos entre llegadas para los trabajos y las tareas, con NEW_JOB y NEW_TASK respectivamente.
Consideraremos también una probabilidad 0.2 para que una máquina haya de ser reparada después de hacer un trabajo (CHECK_WORN).


```r
# inicializamos una semilla para reproducir siempre las mismas simulaciones
set.seed(1234)
# e inicializamos el entorno de simulación
env=simmer("JobShop")

# Definimos cómo se simularán los tiempos de las actividades
RUNNING=function() rexp(1,1)
RETOOL= function() rexp(1,2)
RESET= function() rexp(1,3)
AWAY= function() rexp(1,1)
# chequeo de desgaste y necesidad de reparación
CHECK_WORN=function() runif(1)<0.2 # da FALSE/TRUE
# y las llegadas de trabajos y tareas personales
NEW_JOB=function() rexp(1,5)
NEW_TASK=function() rexp(1,1)
```

La trayectoria de un trabajo ('job') que llega a la tienda, empieza por ocupar (seize) una máquina que está preparada (estado READY).
La máquina opera durante cierto tiempo aleatorio (RUNNING) en el que está resolviendo un trabajo.
Cuando la máquina finaliza este tiempo, se chequea (CHECK_WORN) para comprobar si hay que cambiar piezas o no.
El chequeo se realiza a través de una ramificación en la que si hay que reparar (con probabilidad 0.2), la máquina accede a la reparación que lleva a cabo el operario durante un tiempo RETOOL; si no, continua con la reinicialización durante un tiempo RESET, también desarrollada por el operario.
Finaliza el servicio con la compleción del trabajo del operario y del trabajo de la máquina.

Por otro lado, las tareas personales que realizan los operadores, los mantienen ocupados durante un tiempo AWAY.


```r
job=trajectory() %>%
  seize("máquina") %>%
  timeout(RUNNING) %>%
  branch(
    CHECK_WORN, continue=TRUE, 
      trajectory() %>%
        seize("operario") %>%
        timeout(RETOOL) %>%
        release("operario")
  ) %>%
  seize("operario") %>%
  timeout(RESET) %>%
  release("operario") %>%
  release("máquina")

task=trajectory() %>%
  seize("operario") %>%
  timeout(AWAY) %>%
  release("operario")
```

Una vez que han sido definidas las trayectorias de los procesos, de las máquinas y de los operarios, dimensionamos el sistema con 10 máquinas idénticas y 5 operarios, y creamos también un generador de trabajos (jobs) y otro de tareas personales (tasks).
Dejamos correr el sistema durante 1000 unidades de tiempo.


```r
env %>%
  add_resource("máquina",10) %>%
  add_resource("operario",5) %>%
  add_generator("job",job,NEW_JOB)%>%
  add_generator("task",task,NEW_TASK) %>%
  run(until=1000) %>%
  invisible
```

El simulador monitoriza en pantalla todos los cambios de estado y tiempos de vida de todos los procesos, lo que nos permite realizar cualquier tipo de análisis sin demasiado esfuerzo adicional.
Por ejemplo, podríamos extraer el histórico de los estados de los recursos para analizar el número medio de máquinas/operarios utilizados, así como el número de trabajos/tareas esperando ser asignados.


```r
aggregate(cbind(server,queue)~resource, get_mon_resources(env),mean)
```

```
##   resource   server     queue
## 1  máquina 7.987438 1.0355590
## 2 operario 3.505732 0.4441298
```

### Sistemas de colas

Veamos cómo implementar con `simmer` una cola M/M/1 según el ejemplo mostrado en [FishyOperations, 2016](https://www.r-bloggers.com/2016/04/simulating-queueing-systems-with-simmer/).
Las viñetas de simmer incluyen más ejemplos de sistemas M/M/c/k ([Ucar, 2020a](https://r-simmer.org/articles/simmer-06-queueing.html)), redes de colas y modelos de Markov de tiempo continuo CTMC ([Ucar, 2020b](https://r-simmer.org/articles/simmer-07-ctmc.html)).

En la notación de Kendall (Kendall, 1953), un sistema M/M/1 tiene una distribución de llegadas exponencial con media $\lambda$, (**M**/M/1), un único servidor (M/M/**1**), y un tiempo de servicio exponencial de media $\mu$, (M/**M**/1).
Por ejemplo, la gente llega a un cajero automático aproximadamente cada $\lambda$ minutos, espera su turno en la calle y saca dinero durante aproximadamente $\mu$ minutos.
Se definen entonces los parámetros básicos del sistema cuando $\rho<1$ (para que no se sature el sistema):

-   utilización del servidor, $\rho=\lambda/\mu$
-   promedio de clientes en el sistema (cola y cajero), $N=\rho/(1-\rho)$
-   tiempo medio en el sistema (ley de Little), $T=N/\lambda$.

Si $\rho>1$, el sistema es inestable pues hay más llegadas de las que el servidor es capaz de gestionar y la cola crecerá indefinidamente.

La simulación de este sistema con simmer es muy sencilla.


```r
set.seed(1234)
lambda=2
mu=4
rho=lambda/mu

mm1.cajero=trajectory() %>%
  seize("cajero",amount=1) %>%
  timeout(function() rexp(1,mu)) %>%
  release("cajero",amount=1)

mm1.env=simmer() %>%
  add_resource("cajero",capacity=1,queue_size=Inf) %>%
  add_generator("cliente",mm1.cajero,function() rexp(1,lambda)) %>%
  run(until=2000) 
```

Después de la especificación de los parámetros ($\lambda$, $\mu$), el primer bloque de código define la trayectoria: cada llegada será asignada al recurso, sacará dinero durante un tiempo variable que responderá a una distribución exponencial con media $\mu$ y dejará el cajero.

El segundo bloque de código hace una instancia al entorno de simulación, crea el recurso, define el generador de llegadas de clientes y lanza la simulación durante 2000 unidades de tiempo.

A continuación visualizamos los datos simulados de funcionamiento del sistema.
En el primer gráfico, la evolución temporal de los recursos: cola y servidor.
En el segundo gráfico el tiempo de espera a lo largo del periodo de simulación, y en el tercer gráfico el tiempo de servicio a lo largo del periodo de simulación.


```r
resources=get_mon_resources(mm1.env)
arrivals=get_mon_arrivals(mm1.env)
plot(resources, metric="usage", "cajero", items = c("server","queue"))
```



\begin{center}\includegraphics[width=0.95\linewidth]{07-simmer_files/figure-latex/unnamed-chunk-35-1} \end{center}

```r
plot(arrivals, metric="waiting_time")
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```



\begin{center}\includegraphics[width=0.95\linewidth]{07-simmer_files/figure-latex/unnamed-chunk-35-2} \end{center}

```r
plot(arrivals, metric="activity_time")
```

```
## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'
```



\begin{center}\includegraphics[width=0.95\linewidth]{07-simmer_files/figure-latex/unnamed-chunk-35-3} \end{center}

Si queremos inferir sobre el funcionamiento del sistema a largo plazo, necesitaremos muestras, obtenidas de varias réplicas.
La replicación es recomendable llevarla a cabo con la librería *parallel*, que permite lanzar en paralelo varias réplicas simultáneas.


```r
#library(parallel)
mm1.envs=mclapply(1:100,function(i) { 
     simmer() %>%
        add_resource("cajero",capacity=1,queue_size=Inf) %>%
        add_generator("cliente",mm1.cajero,function() rexp(100,lambda)) %>%
        run(until=1000/lambda) %>%
       wrap()
     }, mc.set.seed=FALSE)
```

Al recuperar las llegadas con `get_mon_arrivals`, obtenemos un data.frame con todas las llegadas, el instante de llegada (*start_time*) y salida (*end_time*), así como el tiempo en servicio -sin contar espera- (*activity_time*), un indicador de si ha finalizado la actividad (*finished*) y un índice de replicación (*replication*).


```r
mm1.arrivals=get_mon_arrivals(mm1.envs) 
head(mm1.arrivals)
```

```
##       name start_time  end_time activity_time finished replication
## 1 cliente0  0.3373840 0.3988817    0.06149772     TRUE           1
## 2 cliente1  0.8873295 1.0936262    0.20629668     TRUE           1
## 3 cliente2  1.0175261 1.3095587    0.21593254     TRUE           1
## 4 cliente3  1.2768201 1.7538044    0.44424568     TRUE           1
## 5 cliente4  1.6875986 1.9712527    0.21744829     TRUE           1
## 6 cliente5  1.8805297 2.1871458    0.21589316     TRUE           1
```

Con varias réplicas podríamos, por ejemplo, calcular el tiempo medio en el sistema (*end_time-start_time*), y con todas las medias, testar si el tiempo medio es superior o inferior a cualquier valor que marquemos, utilizando en este caso un test t-Student:


```r
mm1.data=get_mon_arrivals(mm1.envs) %>%
  group_by(replication) %>%
  summarise(m=mean(end_time-start_time))
ggplot(mm1.data,aes(x=replication,y=m))+
  geom_point()+
  geom_line()
```



\begin{center}\includegraphics[width=0.95\linewidth]{07-simmer_files/figure-latex/unnamed-chunk-38-1} \end{center}

```r
t.test(mm1.data$m,mu=0.5,alternative ="greater")
```

```
## 
## 	One Sample t-test
## 
## data:  mm1.data$m
## t = -0.51444, df = 99, p-value = 0.696
## alternative hypothesis: true mean is greater than 0.5
## 95 percent confidence interval:
##  0.4906473       Inf
## sample estimates:
## mean of x 
## 0.4977877
```

O estudiar la utilización de los recursos,


```r
mm1.resources=get_mon_resources(mm1.envs)
plot(mm1.resources, metric="utilization")
```



\begin{center}\includegraphics[width=0.95\linewidth]{07-simmer_files/figure-latex/unnamed-chunk-39-1} \end{center}

O incluso testar si el tamaño de la cola (o de sujetos en el sistema) ha sido cero (o cualquier otro valor),


```r
mm1.data=get_mon_resources(mm1.envs) %>%
  group_by(replication) %>%
  summarise(n.cola=mean(queue),n.sistema=mean(system))
ggplot(mm1.data,aes(x=replication,y=n.cola))+
  geom_point()+
  geom_line()+
  geom_point(aes(y=n.sistema),color="blue")+
  geom_line(aes(y=n.sistema),color="blue")+
  labs(x="Replicación",y="Usuarios",caption="Figura x. Usuarios en cola (negro) y en el sistema (azul).")
```



\begin{center}\includegraphics[width=0.95\linewidth]{07-simmer_files/figure-latex/unnamed-chunk-40-1} \end{center}

```r
t.test(mm1.data$n.cola,mu=0,alternative = "greater")
```

```
## 
## 	One Sample t-test
## 
## data:  mm1.data$n.cola
## t = 72.742, df = 99, p-value < 2.2e-16
## alternative hypothesis: true mean is greater than 0
## 95 percent confidence interval:
##  0.7300316       Inf
## sample estimates:
## mean of x 
## 0.7470844
```

```r
t.test(mm1.data$n.sistema,mu=1,alternative = "greater")
```

```
## 
## 	One Sample t-test
## 
## data:  mm1.data$n.sistema
## t = 44.214, df = 99, p-value < 2.2e-16
## alternative hypothesis: true mean is greater than 1
## 95 percent confidence interval:
##  1.477677      Inf
## sample estimates:
## mean of x 
##  1.496315
```

### Gasolinera

El ejemplo de la gasolinera es ofrecido en [Ucar (2020b)](https://r-simmer.org/articles/simmer-07-ctmc.html) como una ejemplificación de un proceso de cadena de markov continuo en el tiempo.
Una gasolinera tiene un único surtidor y no dispone de espacio de espera para los vehículos (si un vehículo llega mientras otro está utilizando el surtidor, ha de marcharse).
Los vehículos llegan a la gasolinera según un proceso de Poisson de razón $\lambda=3/20$ vehículos por minuto.
De los vehículos que llegan, el 75% son coches y el resto motocicletas.
El tiempo de carga de combustible responde a una distribución exponencial con media 8 minutos para los coches y 3 para las motocicletas.

Recordemos que un proceso de Poisson de parámetro $\lambda$ implica una distribución de Poisson $Po(\lambda t)$ para el número de usuarios del sistema transcurrido un tiempo $t$ y una distribución $Exp(\lambda)$ para los tiempos entre llegadas de los usuarios.

Puesto que se trata de un proceso continuo en el tiempo, definimos una función que ejecuta el proceso hasta un instante $t$.
El único surtidor de la gasolinera constituye el recurso del proceso, con capacidad 1 y tamaño de la cola 0, y las llegadas de vehículos se generan según una distribución $Exp(\lambda)$.
Al producirse una llegada, se deriva al surtidor, especificando `amount=1` para provocar que si está ocupado, el vehículo se marcha (es rechazado).
Puesto que el tiempo de recarga de combustible depende de si el vehículo es coche o motocicleta, y estos llegan en una proporción 3 a 1 (75% coches-25% motos), el tiempo de permanencia en el surtidor especificado con `timeout` se genera con probabilidad 0.75 con una $Exp(1/8)$ y con probabilidad 0.25 con una $Exp(1/3)$.
Transcurrido ese tiempo, el vehículo abandona el sistema.


```r
 recarga<- function(t) {
  vehiculo <- trajectory() %>%
    seize("surtidor", amount=1) %>%
    timeout(function() {
      if (runif(1) < p) rexp(1, mu[1])  # coche
      else rexp(1, mu[2])               # moto
    }) %>%
    release("surtidor", amount=1)

simmer() %>%
    add_resource("surtidor", capacity=1, queue_size=0) %>%
    add_generator("vehiculo", vehiculo, function() rexp(1, lambda)) %>%
    run(until=t)
 }
```

Definiendo los valores de los parámetros, hacemos correr el sistema hasta el instante $t=5000$, y visualizamos llegadas y recurso.


```r
t=5000
lambda=3/20
mu=c(1/8,1/3)
p=0.75
 
gasolinera=recarga(t) 
llegadas=get_mon_arrivals(gasolinera)
surtidor=get_mon_resources(gasolinera)
plot(llegadas, metric="activity_time")
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```



\begin{center}\includegraphics[width=0.95\linewidth]{07-simmer_files/figure-latex/unnamed-chunk-42-1} \end{center}

```r
plot(surtidor, "usage","surtidor",items="system")+
  geom_hline(yintercept=0.5)
```



\begin{center}\includegraphics[width=0.95\linewidth]{07-simmer_files/figure-latex/unnamed-chunk-42-2} \end{center}

## Referencias

Bache SM, Wickham H (2014).
magrittr: A Forward-Pipe Operator for R.
R package version 1.5.
Available at <https://CRAN.R-project.org/package=magrittr>.

Banks J (2005).
Discrete-Event System Simulation.
Prentice-Hall International Series in Industrial and Systems Engineering.
Pearson Prentice Hall.

Canadilla, P. (2019).
Analysis of Queueing Networks and Models.
Package 'queueing'.
Available at <https://cran.r-project.org/web/packages/queueing/>.

Chubaty AM, McIntire EJB (2019).
SpaDES: Develop and Run Spatially Explicit Discrete Event Simulation Models.
R package version 2.0.3.
Available at <https://CRAN.R-project.org/package=SpaDES>.

Ebert A (2018).
queuecomputer: Computationally Efficient Queue Simulation.
R package version 0.8.3.
Available at <https://CRAN.R-project.org/package=queuecomputer>.

Ebert A, Wu P, Mengersen K, Ruggeri F (2017).
Computationally Efficient Simulation of Queues: The R Package queuecomputer." arXiv:1703.02151. ArXiv.org E-Print Archive. Available at <http://arxiv.org/abs/1703.02151>.

FishyOperations in R-Bloggers (2016).
Simulating queueing systems with simmer.
Available at <https://www.r-bloggers.com/2016/04/simulating-queueing-systems-with-simmer/>.

Kendall DG (1953).
Stochastic Processes Occurring in the Theory of Queues and their Analysis by the Method of the Imbedded Markov Chain." The Annals of Mathematical Statistics, 24(3), 338-354. Available at <https://doi.org/10.1214/aoms/1177728975>.

Lauwens B (2017).
SimJulia.jl: Combined Continuous-Time / Discrete-Event Process Ori-ented Simulation Framework Written in Julia.
Julia package version 0.5.
Available at <https://github.com/BenLauwens/SimJulia.jl>.

Law AM, Kelton WD (2000).
Simulation Modeling and Analysis.
McGraw-Hill Series in Industrial Engineering and Management Science.
McGraw-Hill.

Pidd M (1988).
Computer Simulation in Management Science.
John Wiley & Sons.

Shannon RE (1975).
Systems Simulation: The Art and Science.
Prentice-Hall.

Team SimPy (2017).
SimPy: Discrete-Event Simulation for Python.
Python package version 3.0.9.
Available at <https://simpy.readthedocs.io/en/stable>.

Ucar, I., Smeets, B., Azcorra, A.
(2019).
"simmer: Discrete-Event Simulation for R." *Journal of Statistical Software*, *90*(2), 1-30.
Available at <https://doi.org/10.18637/jss.v090.i02>.

Ucar, I., Smeets, B.
(2019a).
simmer: Discrete-Event Simulation for R.
R package version 4.3.0.
Available at <https://CRAN.R-project.org/package=simmer>.

Ucar, I., Smeets, B.
(2019b).
simmer.plot: Plotting Methods for simmer.
R package version 0.1.15.
Available at <https://CRAN.R-project.org/package=simmer.plot>.

Ucar, I., Smeets, B.
(2020c) simmer: DES for R.
Available at <https://r-simmer.org/>.
Documentation is available at <https://r-simmer.org/reference/>.

Ucar, I.
(2020a).
Queueing systems, in simmer: DES for R.
Available at <https://r-simmer.org/articles/simmer-06-queueing.html>.

Ucar, I.
(2020b).
Continuous-Time Markov Chains, in simmer: DES for R.
Available at <https://r-simmer.org/articles/simmer-07-ctmc.html>.
