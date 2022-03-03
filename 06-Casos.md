# Aplicaciones prácticas {#SIMSIST}

En esta unidad se presentan diferntes sistemas d eproducción sobre los que hay que diseñar un algoritmo de simulación para responder a las preguntas de interés.

**Caso 1.** Trabaja en una empresa que da servicio de distribución de aguas. Concretamente se le ha encargado que preste su atención del departamento de atención telefónica. Actualmente se reconocen 3 tipos de llamadas que se reciben en tres teléfonos distintos. De tipo 1 se reciben 40 llamadas a la hora, de tipo 2 se reciben también 40 llamadas a la hora y de tipo 3 se reciben en condiciones normales 20 llamadas a la hora. 

El tiempo que se tarda en atender una llamada de tipo 1 es de 3 minutos igual que las llamadas de tipo 2. Las llamadas de tipo 3 requieren una atención en promedio de 5 minutos cada una. Usted está diseñando un nuevo sistema de atención telefónica, que atendería a todos los clientes con un único número de teléfono. Un sistema informático discrimina el destino de la llamada mediante una operación que dura aproximadamente 20 segundos en promedio. Una vez el sistema informático decide el destino, tiene una probabilidad del 5% de equivocarse. En ese caso el operador que recibe la llamada, envía ésta al centro adecuado para que sea atendido. La empresa esta interesado en:

* ¿Cuántos operadores se deben poner para cada tipo de llamadas si deseamos mantener la intensidad de tráfico en estado estacionario ($\rho < 1$)?
* ¿Cuál es el tiempo medio que un cliente estaría en el sistema con el número mínimo de operadores? ¿cuál es el tiempo esperado en cola para ser atendido para las llamadas de cada tipo? ¿Cuántas llamadas habría en promedio en cada sección?
* ¿Cuál es tiempo medio que un cliente estaría en el sistema si pusiera en cada sección uno más de los operadores estrictamente necesarios?
* Se le plantea una nueva alternativa. Consiste en hacer que todos los operadores atiendan todas las llamadas, aunque en ese caso el tiempo de atención de cada llamada es el doble del indicado más arriba para cada tipo. ¿Cuántos operadores hacen falta? ¿Cuál es el tiempo medio de estancia en el sistema? ¿Cuál es el tiempo de espera a ser atendido?

**Caso 2.** El servicio técnico de una empresa tiene 3 etapas relevantes y necesarias para todos los productos que maneja. Cada una de ellas es relativamente manual y la capacidad productiva de la misma es directamente proporcional al número de personas que trabajan en la misma. El número de productos que se reciben en dicho servicio técnico es de 41 unidades al día y la llegada de los mismos sigue un
proceso de Poisson. Tras la tercera etapa hay un proceso de control de calidad que revisa el producto
obtenido. Es también una etapa manual y se podría considerar una cuarta etapa. Tras la inspección un cierto porcentaje de productos son devueltos a la etapa 1, otros a la etapa 2 y otros a la etapa 3. Por la configuración del producto, una vez un producto vuelve a la etapa 1 debe seguir el proceso preestablecido hasta el final. 

Los datos de cada etapa están en la tabla adjunta. Los tiempos de operación en cada etapa están expresados en horas y se ajustan razonablemente bien a una distribución exponencial:

$$
\begin{matrix}
& Etapa 1 & Etapa 2 & Etapa 3 & Calidad\\
\text{Tiempo de opración (en horas)} & 3 & 4 & 2 & 1\\
\text{Porcentaje devueltos} & 2\% & 3\% & 5\% &
\end{matrix}
$$

Los productos que se fabrican son específicos para cada cliente. El cliente tiene una cierta urgencia en recoger su producto acabado y por ello el tiempo de espera del mismo es un tema relevante. Si la empresa está interesada en la producción para los próximos 6 meses (cada día tiene 7 horas y 30 minutos de trabajo efectivo, y cada mes 22 días de trabajo.)

* Diseñe el sistema que con un mínimo número de personas total cumple con los requerimientos.
* Defina los parámetros básicos del sistema: número de pedidos promedios, tiempo de espera medio, numero medio de pedidos por etapa.

Los datos de tiempo de espera parecen muy elevados. Se le ocurren varias maneras de atacar el problema:

* Contratar una persona más. Si tuviera que proponer la contratación de una persona más ¿dónde la pondría y por qué? ¿qué efecto tendrá sobre el sistema?
* Invertir en alguna de las diferentes etapas para reducir a la mitad la tasa de fallos. ¿en cuál y por qué? ¿qué efecto tendría en el sistema?
* Invertir en alguna de las diferentes etapas para reducir a la mitad la variabilidad del proceso. ¿en cuál y por qué? ¿qué efecto tendría? 

**Caso 3.** Acaba usted de llegar a la cola del aparcamiento de un parque temático. Las colas empiezan con los cajeros a los que paga 6 Euros por coche que entra en el aparcamiento. La segunda cola empieza mientras espera que le asignen una plaza del aparcamiento. La tercera es la cola para pagar la entrada ( a 8 euros más por persona), y por último una cola para que comprueben que no pretende entrar comida en el parque temático. Una vez dentro del Parque comenzará una serie de colas que acabará en cada una de las atracciones. No todo el mundo que entra en el Parque sigue el mismo camino, pero su interés radica en saber cuánto tardará usted en alcanzar el interior del Parque. 

Un breve análisis de la situación indica los siguientes datos:

* Entran aproximadamente 2400 coches por hora, para ser atendidos por 20 cajas en paralelo que tardan en cobrar aproximadamente 29 segundos por cliente. 
* De todos los coches que entran un 18% van al aparcamiento VIP. El 82% restante va al aparcamiento convencional que, de un modo muy eficiente es capaz de aparcar los coches, de uno en uno, con un tiempo de ciclo promedio de 3,5 segundos por coche. Mediante otros medios (trenes y autobuses) se acercan junto con los clientes en coche particular nuevos grupos de clientes.
* Se calcula que en el parque entran aproximadamente 25.000 personas al día en 6000 grupos. De los 6000 grupos sólo 3500 grupos compran las entradas en taquilla (los otros ya las compraron por agencia o llevan un pase de varios días comprado anteriormente). Todos los clientes llegan aproximadamente en las 3 primeras horas de apertura del parque.  
* Los que han de pagar tendrán que hacer 6 colas para pagar en 30 cajas (cada cola alimenta a 5 cajas). En cada caja tardarán en promedio 92 segundos en atenderles. 
* Tras pagar queda la última cola donde cada cliente pasa de modo individual, y pasan todos: los que acaban de comprar y los que venían con ticket precomprado, por el detector de comidas y bebidas. Estos son 12 carriles en paralelo, cada uno con su propia cola, que tardan 5 segundos en dejar pasar a cada cliente.

Para un día normal de funcionamiento:

* Si cada coche mide 4 metros, cuantos metros de carretera hacen falta para que quepan en promedio todos los coches que se pondrán en cola delante de las cajas de aparcamiento.
* ¿Cuánto tiempo se tarda en hacer la cola para aparcar el coche, una vez haya pagado el aparcamiento?
* ¿Cuánto tiempo tardaremos en conseguir nuestra entrada desde que hemos aparcado el coche, considerando que tarda 5 minutos desde que aparca hasta que llega a la cola?
* ¿Cuánta gente habrá como usted haciendo cola para pagar?
* ¿Cuánto tiempo tardaremos en entrar en el parque desde que aparcamos?.
* ¿qué ocurrirá en el sistema de cajas si el tiempo de atención en la caja de compra de entradas el tiempo medio de atención es de 100 segundos?
* ¿qué repercusión tendría en la cola posterior dicha alteración?
* Con las condiciones de e, ¿cuál es la repercusión de añadir un carril adicional de venta de entradas?

**Caso 4.** JCP Automatismos es una empresa que diseña, fabrica e instala sistemas de manutención automáticos. Actualmente se encuentran diseñando un sistema que pretenden dimensionar apoyándose en sus conocimientos. Dicho sistema recoge, en una de sus partes, tres tipos de productos paletizados a través de sendas mesas transportadoras de rodillos. Dichas mesas transportadoras desembocan en un carro. El carro recoge las paletas que las mesas le suministran y las envía a otras dos mesas de rodillos que alimentan sendas máquinas.

El ritmo de entrada de productos 1 en su mesa es de 20 paletas/hora. El de productos de tipo 2 es 40 paletas/hora. El de productos de tipo 3 es 60 paletas/hora.

El tiempo que en promedio tarda el transportador en cubrir un ciclo entero es de 25 segundos por paleta transportada. Es decir en promedio se tarda 25 segundos en moverse a la mesa de rodillos en la que hay que recoger el producto, coger la paleta, desplazarse a la mesa de destino y descargar la paleta. El transportador elije el producto a transportar sin seguir ningún criterio específico. El 80% de los productos que entran por 1 va a la mesa a. El 40% de los productos que entran por 2 van a la mesa a. El 50% de los productos que entran por 3 van a la mesa a. La máquina que consume elementos de la mesa a lo hace a un ritmo de 50 segundos por paleta. La máquina que consume elementos de la mesa b lo hace también a 50 segundos por paleta.

Se trata de definir la capacidad mínima que han de tener las 5 mesas de rodillos, para que el sistema no se bloquee. Para ello:

* Definir el problema según una red de colas de varios productos (suponer que las colas inicialmente no tienen límite en la capacidad). Definir la matriz de transición para cada producto.
* Calcular los valores de $\lambda$ y $\mu$ para cada una de los servidores.
* ¿Cuál será la cola promedio en cada una de las mesas suponiendo que no tienen límite de capacidad?
* ¿Cómo afectaría al sistema que el transportador eligiera para extraer, en cada ocasión, el producto en cabecera de la mesa de rodillos con más productos?
* ¿Cómo cree que afectará a la máquina que alimenta a la mesa 3 que ésta tuviera una limitación de 10 unidades?. ¿Y si fuera de 5 unidades? 

**Caso 5.** Ascensores PKJu es una empresa de mantenimiento de ascensores que opera en el área de Elche. Tiene a su cargo un parque de 500 ascensores. Usted se está buscando la vida como “consultor de Organización”. Y en el edificio donde viven ha sufrido ya en dos ocasiones un servicio bastante defectuoso
–tardaron más de dos días en resolver un problema-, así que se ha ido a ofrecerle a la dirección de la empresa la posibilidad de repensar el modo de funcionamiento.

En la empresa son conscientes de que efectivamente tienen un problema (las reclamaciones de los clientes indican que algo pasa). Le han pedido que haga una propuesta de mejora. Las actividades de mantenimiento a los ascensores son de tres tipos:

* Las actividades de mantenimiento preventivo (aproximadamente 1,1 revisiones al año por ascensor). La duración estándar de la operación oscila entre 3 y 5 horas (incluyendo desplazamientos que son en promedio una hora y media todo incluido).
* Las actividades de mantenimiento correctivo que suponen alrededor de 500 acciones al año (estas actividades tienen un tiempo promedio de 4 horas de resolución).
* Las actividades de emergencia (donde hay que sacar a alguien de un ascensor bloqueado o similar) que suponen alrededor de 100 acciones al año y que tienen un tiempo de resolución promedio de dos horas. El 75% de las mismas ocurren durante el tiempo “de oficina”. Si de resultas de la actividad de emergencia hay que resolver algo más (lo que ocurre en el 50% de las ocasiones) se hace en el momento como una
actividad de mantenimiento correctivo.

Tienen tres técnicos “de toda la vida” que se desenvuelven mejor en las actividades de mantenimiento correctivo (tardando aproximadamente un 20% menos de tiempo en resolver cualquier problema que el promedio) pero no les gustan las de preventivo (“porque ahora todo lo hacen los ordenadores”, es por ello
que siempre encuentran motivos para estar un 20% más del tiempo previsto).

Conocen a suficientes técnicos de ascensores para saber que puede contar con los que necesite sin experiencia. Actualmente, cuando creen que van a necesitar más gente, la contratan por semanas, pero calculan que les sale alrededor de un 20% más cara que si el contrato fuera fijo. 

Para las emergencias han creado un retén que trabaja 24 horas al día, 365 días al año. El año tiene 240 días laborables. Y los días tienen 8 horas laborables. Como el retén tiene que estar disponible 24/365 la empresa ha subcontratado por un fijo a dos autónomos que dan ese servicio durante las horas que no son “de oficina”.

Su modo de gestión en los días laborables y en horas de oficina” es el siguiente. Cada día laborable se inician actividades de mantenimiento según estén programadas. Uno de los trabajadores experimentados se queda en el retén por si hay alguna emergencia. En el caso de que haya que atender una emergencia se
envía al técnico que está en el retén al lugar de “los hechos”. Si no hubiera ninguno disponible en el retén se localiza a algún otro técnico para que se desplace al lugar donde es necesario urgentemente.

Actualmente tiene un tipo de gestión en que cualquier técnico está disponible para recibir una orden de trabajo desde cualquier ascensor. Se le ha ocurrido que sería mejor dividir su zona en varias áreas. De tal modo que los técnicos se especialicen por zonas (así incrementan el conocimiento relativo de su parque de ascensores). Estima que el beneficio que obtendría con ello es que reduciría tiempos de desplazamiento en al menos un 50%. Se trata de que exponga, de la manera más clara y concisa posible cómo plantearía el problema a la empresa. Cuanto más nivel de detalle le dé a la solución mejor.

**Caso 6.** Un pequeño supermercado de playa tiene tres líneas de cajas. El propietario cuenta con la ayuda de dos auxiliares. El propietario es mucho más eficiente que los auxiliares siendo capaz el primero
de atender a un cliente cada 3 minutos, mientras que el auxiliar necesita 4 minutos.

En los momentos de mayor afluencia de gente entran alrededor de 30 clientes a la hora, un 20% de los cuales tarda 5 minutos en irse sin comprar nada si el propietario está atendiendo la caja. Ese porcentaje se reduce al 10% si el propietario está paseando por el interior de la tienda. Los clientes que compran algo tardan en promedio 6 minutos en pasar desde la “sala” a la zona de cajas.

A los clientes les disgusta estar en la cola. Un 15% de los mismos no entran en el supermercado si ven demasiado “lío” en la caja.

En general al propietario le gusta estar al tanto de los lineales y por ello deja a uno de los auxiliares al cargo de la caja. Si la cola crece por encima de 5 pone al segundo de los auxiliares y sólo cuando ve que la cola se hace demasiado larga (más de 10 personas aproximadamente) abre la otra caja y ayuda hasta que quedan 5 personas en la cola), momento en el que se va.  

Para un día cualquiera (12 horas de funcionamiento):

* ¿Cuánta gente hay en el supermercado en los momentos de mayor afluencia?
* ¿Hace bien el propietario en ponerse en la cola sólo cuando los dos auxiliares están en la caja y ve que la cola sigue siendo demasiado larga?
* ¿Cuál es la longitud exacta de la cola en el caso de que el propietario de la caja se incorpore exactamente cuándo hay 10 clientes en la cola y abandone cuando sólo quedan 5?


