---
title: "Simulación de Procesos y Sistemas"
author: 
  - name: "Asunción M. Mayoral (asun.mayoral@umh.es). [Web personal](https://asunmayoral.umh.es)"
    affiliation: "[Instituto Universitario de Investigación CIO-UMH](https://cio.umh.es)"
  - name: "Javier Morales (j.morales@umh.es). [Web personal](https://sites.google.com/goumh.umh.es/javier-morales)"
    affiliation: "[Instituto Universitario de Investigación CIO-UMH](https://cio.umh.es)"
date: "2022-04-26"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
biblio-style: "apacite"
link-citations: TRUE
description: "Manual práctico sobre simulación de procesos y sistemas"
csl: chicago-fullnote-bibliography.csl
---



# Antes de comenzar {.unnumbered}

La simulación es una de las herramientas de modelización probabilística más utilizadas en la industria. Se utiliza para el análisis de sistemas existentes y para la selección de sistemas óptimos a partir del planteamiento y comparación de diversos escenarios plausibles.

::: {.whitebox data-latex=""}
::: example
Por ejemplo, supongamos que un gran supermercado ha estado recibiendo quejas de los clientes sobre el tiempo que pasan en la cola esperando una caja disponible para pagar. La dirección ha decidido añadir algunas cajas más, pero ha de decidir cuántas añadir. Los modelos de simulación pueden ayudar a la dirección a determinar el número de cajas necesarias para dar un servicio adecuado a sus clientes.
:::
:::

Aunque el enfoque principal de la Estadística es la construcción de modelos analíticos que describan el funcionamiento de los procesos en función de variables que les afecten, en ocasiones nos puede resultar menos costoso proceder mediante simulación para obtener una predicción razonable de cómo se va a comportar el sistema o proceso ante distintas configuraciones de esas variables condicionantes. La simulación permitirá obtener una aproximación del comportamiento del sistema ante distintos escenarios.

::: {.whitebox data-latex=""}
::: example
La forma de dar una solución al problema del supermercado mediante simulación podría consistir en escribir un programa informático que genere clientes que lleguen aleatoriamente (con la frecuencia habitual con que lo hacen, o con otras), simular también unos tiempos de permanencia en caja (para realizar los pagos) y evaluar los tiempos de espera para distintas configuraciones del número de cajas abiertas. Se podría así determinar el efecto de tener varias cajas abiertas en función del tráfico de clientes, y así mismo dimensionar con antelación su plantilla, quizás incluso adaptándola a diferentes épocas o días, para tener suficientes cajeros ubicados en las cajas.
:::
:::

Una razón importante para justificar el uso de la simulación es que puede utilizarse para aumentar la comprensión de un proceso. Es imposible construir un modelo de simulación de un proceso que no se entiende cómo funciona, con lo que el mero hecho de desarrollar un modelo de simulación de un proceso especíﬁco forzará a comprenderlo.

Sin embargo, el aspecto más relevante a tener en cuenta al construir un modelo de simulación es que este reproduzca la realidad fielmente, o al menos de la forma más precisa posible. En problemas sencillos, como los que veremos al inicio de este manual, resulta fácil comprobar si los algoritmos de simulación dan buenas soluciones; sin embargo, en sistemas más complejos no es trivial asegurar la calidad de las simulaciones obtenidas al reproducir el funcionamiento del sistema que simulan, por lo que será necesario establecer medidas de validación.

Para poder abordar sistemas de simulación es necesario tener previamente unos conocimientos básicos de probabilidad, que desarrollamos en la Unidad 1 [Conceptos básicos](#intro), y a partir de los que damos la definición de 'proceso estocástico'. Seguimos el camino en la Unidad 2 [Cadenas de Markov de Tiempo Discreto](#cmtd) con el estudio de las cadenas de Markov de tiempo discreto, los procesos de Poisson en la Unidad 3 [Proceso de Poisson](#poissonprocess), las cadenas de Markov de tiempo continuo en la Unidad 4 [Cadenas de Markov de Tiempo Contínuo](#CMTC), y finalizamos con las colas de espera en la Unidad 5 [Sistemas de colas](#COLAS).

Al final de todas las unidades se proporciona una colección de ejercicios relacionados con los procedimientos estudiados en dicha unidad, para practicar los conceptos estudiados resolviendo problemas generalmente relacionados la realidad. Los ejercicios aparecen etiquetados como **B** (Básicos), de aplicación directa de conceptos o técnicas, o como **A** (Avanzados), si requieren de una modelización concreta y más tiempo para resolverlos.

Todos los ejercicios deben resolverse mediante simulación. Se recomienda identificar las variables involucradas y sus distribuciones, así como construir el algoritmo de simulación (5000 simulaciones y semilla=12), con suficiente detalle y comentarios. El algoritmo de simulación es conveniente que venga desarrollado en función de los parámetros de entrada del problema, de forma que resulte útil para responder las preguntas formuladas y otras que se puedan plantear ante variaciones de las condiciones iniciales.

Parte de los contenidos de este curso se han obtenido de la documentación de las diferentes librerías de R utilizadas, y de los libros @MayMor2007, @FelmanValdez2010, @Kulkarni2011 y @robertcasella2010. Buena parte de los ejercicios y ejemplos propuestos se han obtenido también de los manuales de @garcia-sabater2016 y @cao2002.

# Software {.unnumbered}

Para poder utilizar el código expuesto en estos materiales es necesario la instalación de los programas R [@R-base], que actúa como lenguaje de programación, y RStudio [@rstudio]. que actúa como interfaz, y que se pueden descargar desde:

-   R: <https://cran.r-project.org/>
-   RStudio: <https://rstudio.com/>

Para crear informes directos a partir del código utilizado al programar en R con RStudio se recomienda RMarkdown [@R-rmarkdown].

A continuación se detallan brevemente las librerías especifícas de R utilizadas en este manual. Conviene tenerlas instaladas y actualizadas todas ellas. El conjunto de librerías útiles en Simulación de Procesos y Sistemas son:

-   **tidyverse**, en @tidyverse2019 y @R-tidyverse: Es una colección de librerías en R para la ciencia de datos, que comparten una misma filosofía, gramática y estructuras de datos y facilita el tratamiento de datos. Para aprender a utilizar estas librerías es recomendable el libro *R for Data Science* de @wickham-grolemund, así como el manual de @grosser18.

-   **simmer**, en @simmer2019, @ucar_smeets, @R-simmer: Es una librería R para la simulación de eventos discretos (DES) orientada a procesos. Diseñado para ser un marco genérico como SimPy o SimJulia, aprovecha la potencia de Rcpp para aumentar el rendimiento y hacer factible el DES en R. Como característica destacable, simmer explota el concepto de trayectoria: un camino común en el modelo de simulación para entidades del mismo tipo. Es bastante flexible y sencillo de utilizar, y aprovecha el flujo de trabajo de encadenamiento/conducción introducido por el paquete `magrittr` (@R-magrittr). También utilizaremos las librerías vinculadas `simmer.plot`, `simmer.optim`, `simmer.json`, y `simmer.mom`.

- **parallell** [@R-simmer] permite acelerar la computación de varias réplicas de simulaciones con `simmer` mediante la computación en paralelo.

-   **markovchain** [@R-markovchain]: Librería de R que proporciona clases, métodos y funciones para manejar fácilmente las Cadenas de Markov de Tiempo Discreto (DTMC), realizando análisis probabilísticos y ajustes.

-   **queueing** [@R-queueing]: Proporciona una herramienta versátil para el análisis de los modelos de colas markovianos basados en el nacimiento y la muerte y de las redes de colas monoclase y multiclase.

-   **queuecomputer**, en @queuecomputer2020 y @R-queuecomputer: Implementación de un método computacionalmente eficiente para simular colas con tiempos de llegada y servicio arbitrarios.

Las versiones de las librerías de R utilizadas son las siguientes:


```{.r .Rchunk}
sessionInfo()
```

```{.Rout}
## R version 4.1.2 (2021-11-01)
## Platform: x86_64-apple-darwin17.0 (64-bit)
## Running under: macOS Big Sur 10.16
## 
## Matrix products: default
## BLAS:   /Library/Frameworks/R.framework/Versions/4.1/Resources/lib/libRblas.0.dylib
## LAPACK: /Library/Frameworks/R.framework/Versions/4.1/Resources/lib/libRlapack.dylib
## 
## locale:
## [1] es_ES.UTF-8/es_ES.UTF-8/es_ES.UTF-8/C/es_ES.UTF-8/es_ES.UTF-8
## 
## attached base packages:
## [1] parallel  stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
##  [1] kableExtra_1.3.4    gridExtra_2.3       sjPlot_2.8.10       rootSolve_1.8.2.3  
##  [5] queuecomputer_1.1.0 queueing_0.2.12     markovchain_0.8.6   diagram_1.6.5      
##  [9] shape_1.4.6         simmer.plot_0.1.17  simmer.bricks_0.2.1 simmer_4.4.4       
## [13] forcats_0.5.1       stringr_1.4.0       dplyr_1.0.8         purrr_0.3.4        
## [17] readr_2.1.2         tidyr_1.2.0         tibble_3.1.6        ggplot2_3.3.5      
## [21] tidyverse_1.3.1    
## 
## loaded via a namespace (and not attached):
##  [1] TH.data_1.1-0      minqa_1.2.4        colorspace_2.0-3   ellipsis_0.3.2     sjlabelled_1.2.0  
##  [6] estimability_1.3   parameters_0.17.0  fs_1.5.2           rstudioapi_0.13    matlab_1.0.2      
## [11] fansi_1.0.3        mvtnorm_1.1-3      lubridate_1.8.0    xml2_1.3.3         codetools_0.2-18  
## [16] splines_4.1.2      knitr_1.38         sjmisc_2.8.9       jsonlite_1.8.0     nloptr_2.0.0      
## [21] ggeffects_1.1.2    broom_0.8.0        dbplyr_2.1.1       effectsize_0.6.0.1 compiler_4.1.2    
## [26] httr_1.4.2         sjstats_0.18.1     emmeans_1.7.3      backports_1.4.1    assertthat_0.2.1  
## [31] Matrix_1.4-1       fastmap_1.1.0      cli_3.2.0          formatR_1.12       htmltools_0.5.2   
## [36] tools_4.1.2        igraph_1.3.1       coda_0.19-4        gtable_0.3.0       glue_1.6.2        
## [41] Rcpp_1.0.8.3       cellranger_1.1.0   vctrs_0.4.1        svglite_2.1.0      nlme_3.1-157      
## [46] insight_0.17.0     xfun_0.30          lme4_1.1-29        rvest_1.0.2        lifecycle_1.0.1   
## [51] MASS_7.3-57        zoo_1.8-10         scales_1.2.0       hms_1.1.1          sandwich_3.0-1    
## [56] expm_0.999-6       yaml_2.3.5         stringi_1.7.6      bayestestR_0.11.5  boot_1.3-28       
## [61] systemfonts_1.0.4  rlang_1.0.2        pkgconfig_2.0.3    evaluate_0.15      lattice_0.20-45   
## [66] tidyselect_1.1.2   magrittr_2.0.3     bookdown_0.26      R6_2.5.1           generics_0.1.2    
## [71] multcomp_1.4-18    DBI_1.1.2          pillar_1.7.0       haven_2.5.0        withr_2.5.0       
## [76] survival_3.3-1     datawizard_0.4.0   performance_0.9.0  modelr_0.1.8       crayon_1.5.1      
## [81] utf8_1.2.2         tzdb_0.3.0         rmarkdown_2.14     grid_4.1.2         readxl_1.4.0      
## [86] webshot_0.5.3      reprex_2.0.1       digest_0.6.29      xtable_1.8-4       RcppParallel_5.1.5
## [91] stats4_4.1.2       munsell_0.5.0      viridisLite_0.4.0
```

Cargamos las librerías de interés que utilizaremos en este manual.


```{.r .Rchunk}
# librerías
library(tidyverse)
library(simmer)
library(simmer.bricks)
library(simmer.plot)
library(parallel)
library(diagram)
library(markovchain)
library(queueing)
library(queuecomputer)
library(rootSolve)
# Librerías de entorno gráfico
library(sjPlot)
library(gridExtra)
library(kableExtra)  # y tablas
```

Configuramos además el tema de los gráficos para que tengan un aspecto más limpio y más fácil de exportar en formato pdf o word. Para ellos utilizamos la función `theme_set()`.


```{.r .Rchunk}
theme_set(theme_sjplot2())
```

**Manuales de referencia**

Se recomiendan los siguientes manuales para trabajar con R, RStudio y las librerías proporcionadas:

-   APS 135: Introduction to Exploratory Data Analysis with R. [@childs19] [Versión electrónica](https://dzchilds.github.io/eda-for-bio/).

-   R for Data Science. [@wickham-grolemund] [Acceso web](http://r4ds.had.co.nz/).

-   Advanced R. [@advanced-R] [Versión online](https://adv-r.hadley.nz/) y [Versión en español](https://es.r4ds.hadley.nz/).

-   Tidyverse Cookbook. [@grosser18] [Versión online incompleta](https://bookdown.org/Tazinho/Tidyverse-Cookbook/).

-   ggplot2, part of the tidyverse [@R-ggplot2] [Acceso web](https://ggplot2.tidyverse.org/).

-   RMarkdown básico [@Goicoa17]. [Acceso web](http://www.unavarra.es/personal/tgoicoa/ESTADISTICA_RMarkdown_tomas/basicRmarkdown/index.html).

-   *RMarkdown Cookbook* [@rmarkdown2020] [Versión online](https://bookdown.org/yihui/rmarkdown-cookbook).


