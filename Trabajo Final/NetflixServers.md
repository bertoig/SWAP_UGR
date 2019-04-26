# Play
### Todo lo que sucede cuando le das al play en Netflix


## Introducción
Netflix parece algo muy sencillo para casi todo el mundo pero, ¿qué sucede desde que le damos al 'play' de nuestra serie favorita hasta que la vemos? ¿cuál es la topología de los servidores de esta compañía?

Bien, para continuar con la respuesta a esas dos preguntas comenzaré con una serie de datos acerca del gigante del streaming:

- Tiene más de 110 millones de subscriptores.
- Está disponible en más de 200 países.
- Obtiene mas de 3.000 millones de dólares por trimestre.
- Añade mas de 5 millones de nuevos subscriptores por trimestre.
- Reproduce más de mil millones de horas de vídeo cada semana (Youtube transmite la misma cantidad pero cada día y Facebook transmite 110 millones diarios, a modo de comparación).
- Su record en 2017 de horas transmitidas en un solo día ha sido 250 millones.
- Representa más del 37% del pico de trafico de internet de los EEUU.
- En 2018 estima el gasto en nuevo contenido en 7.000 millones de dólares.

Después de todos estos datos, nos hacemos una idea del tamaño y la potencia que necesita la compañía en sus servidores. Un buen motivo para profundizar sobre Netflix y no sobre otra compañía cualquiera es que ofrece mucha mas información que sus similares. Con los años, han dado cientos de conferencias al mundo de la informática acerca de su funcionamiento interno. Otra de las razones es que todos hemos utilizado alguna vez su servicio y yo el primero, sentí curiosidad por ver como funciona.


## ¿Un único servicio?
Para comenzar, trataré de explicar como funciona Netflix desde el lado tecnológico con un pequeño ejemplo que encontré en uno de los artículos que adjutno en la bibliografía.
Supongamos que tu app de mapas de confianza accede a tu localización en cada instante y genera un archivo llamado localizaciones.txt con dichas actualizaciones. Luego, decides desarrollar una aplicación con uso de geolocalización que recoja las coordenadas de dicho fichero. Unos cuantos meses despues, los desarrolladores de la aplicación de mapas deciden que a partir de ese instante guardaran los datos en una base de datos interna de la empresa. Todo tu trabajo en tu aplicación se habrá ido a la basura gracias a una decisión de una empresa externa. Imagínemonos ahora que esta aplicación que hemos desarrollado sea Netflix y no una 'miniapp', no podemos hacernos la idea de la cantidad de pérdidas que generaría dicho cambio. Dicha arquitectura es conocida como arquitectura monolítica.

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/monolitica.png)

Netflix en cambio, hace unos diez años marcó el comienzo de una revolución reescribiendo aplicaciones con dicha arquitectura en arquitectura de microservicios. Esta arquitectura se puede explicar sencillamente, a partir de este momento, las aplicaciones no compartirán datos con otras de forma natural (como explicamos en el ejemplo de locallizaciones.txt) ahora, las aplicaciones intercambiarán datos entre ellas mediante una API (application programming interface). Así que, en el caso del cambio anterior, al estar programada nuestra 'app' mediante la API de nuestra aplicación de mapas, no tendremos que cambiar absolutamente nada.

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/microservicios.png)

Una vez explicado el uso de los microservicios cabe comentar que Netflix hace uso de más de 700 para controlar correctamente cada parte de la aplicación: un microservicio que guarda qué has visto, otro encargado del cobro mensual, otro encargado de enviarte el archivo de vídeo correcto para tu dispositivo... Tal es el uso de dichos microservicios en Netflix que crearon su propia herramienta para orquestar microservicios: Conductor y su propio centro de software libre debido a sus grandes aportaciones: Netflix OSS.

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/conductor.png) ![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/oss.png)


## ¿Dónde encontramos todo esto?
Netflix nace en 1998 como servicio de alquiler de DVDs a través del servicio postal de EEUU. No fue hasta el 2007 cuando lanzó su servicio de streaming a la carta. En el momento justo, cuando internet era lo suficientemente potente como para servirnos el contenido de una forma bastante buena.

En su comienzo, Netflix crea dos centros de datos, uno al lado del otro en los que no dejan de tener problemas hasta llegar a una  pérdida muy amplia de datos en sus bases de datos relacionales. Por lo tanto, como leemos en un comunicado de Yury Izrailevsky (uno de los encargados de la migración del sistema) en el Netflix Media Center en 2016, tras 7 años de trabajo, consiguieron migrar el sistema a Amazon Web Services, esto les aportó numerosas ventajas, entre otras el aumento de actividad en la empresa como vemos en el siguiente gráfico:

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/streaminghours.png)

A contrinuación seleccioné unas palabras de Yury Izrailevsky en el comunicado en las cuales explica las principales ventajas y las metas que AWS ayudó a conseguir en Netflix:

*Dependemos de la nube para todo lo que necesitamos en cuanto a informática escalable y espacio de almacenamiento: nuestra lógica empresarial, las bases de datos distribuidas y el procesamiento y análisis de grandes datos, las recomendaciones, la transcodificación y otros centenares de funciones que conforman la aplicación de Netflix. El video se transmite a través de Netflix Open Connect, nuestra red de entrega de contenidos que se distribuyen globalmente para enviar de manera eficaz nuestros bits a los dispositivos de los miembros.

La nube también nos ha permitido aumentar de manera significativa la disponibilidad de nuestro servicio. En nuestros centros de datos a veces sufríamos apagones y, aunque pasamos algunos momentos difíciles inevitables en la nube, sobre todo los primeros tiempos de la migración, hemos apreciado un aumento constante en nuestra disponibilidad total, cada vez más cerca de nuestra meta de cuatro nueves (99,99 %) de tiempo de actividad del servicio. Es imposible evitar los fallos en los sistemas distribuidos a gran escala, ni siquiera si están basados en la nube. Sin embargo, la nube permite generar servicios de alta fiabilidad a partir de componentes fundamentalmente no fiables pero redundantes. Al incorporar los principios de la redundancia y degradación correcta a nuestra arquitectura, y al mantener la disciplina de simulacros de producción regulares con Simian Army, podemos sobrevivir a los fallos en la infraestructura de la nube y dentro de nuestros propios sistemas sin afectar la experiencia de los miembros.*

Netflix se volvió mucho más eficaz y eficiente con la mudanza a AWS gracias en gran parte a la redundancia de servidores y datos. Operan con AWS en tres regiones diferentes: dos en EEUU y otra en Irlanda y dentro de cada regíon, utiliza tres zonas de disponibilidad diferentes. Hoy en día afirman que no está en sus planes operar en una nueva región ya que sería demasiado costoso y complicado. Para hacernos una idea, las grandes empresas que usan en este servicio, utilizan una única regíon, muy rara vez dos o incluso tres (otro dato que nos indica lo grande que es la empresa). La ventaja de tener 3 regiones es que si una de ellas falla, las otras dos siguen funcionando correctamente (dentro de la empresa, migrar el tráfico a las regiones no caídas, es denominado 'evacuar una region').

¿Cada cuánto cae una región? Bueno, realizan pruebas para comprobar la correcta evacuación de la región unba vez al mes, por lo tanto, estos mecanismos son bastante utilizados. Llegado el momento, el servicio de la compañía está preparado para ser evacuado en 6 minutos, lo que parece casi de record. Lo mejor de todo es que AWS no ofrece un servicio de "evacuación" si no que todo ello es parte de los ingenieros de Netflix.

Una vez llegados a este punto, comprendiendo el funcionamiento de gran parte de Netflix, conviene cambiar la pregunta.

## ¿Qué sucede en AWS cuando le das al play?
Todo lo que no tiene que ver con la transmisión de vídeo se lleva a cabo en AWS, esto incluye computación escalable, almacenamiento escalable, lógica empresarial, bases de datos descentralizadas escalables, procesamiento y análisis de big data, recomendaciones, transcodificación y cientos de otras funciones.

Podemos dividir la escalabilidad de AWS en dos: EC2 y S3. EC2 es el centro de AWS para la computación escalable y S3 para el almacenamiento. Es decir, cada vez que nos conectamos a Netflix, estamos en continuo contacto con el EC2 de Amazon.

En cuanto al almacenamiento en BBDD, Netflix opta por DynamoDB (propio de Amazon) y Cassandra (clave-valor, de Apache y escrita en Java). Una vez más, Netflix nos explica como funcionan sus bases de datos en The Netflix Tech Blog. Adjunto a continuación el enlace de dicha explicación: https://medium.com/netflix-techblog/implementing-the-netflix-media-database-53b5a840b42a. Resumiendo, nos aportan la siguiente imagen de como almacenan datos:

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/DB.png)

y la siguiente imagen del sistema de almacenamiento NetflixMediaDB:

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/DBsys.png)

![gif](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/DBAccion.gif)

 así como una imagen explicativa acerca de la estrategia de escalado en sus bases de datos:

 ![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/Scaling.png)

 Yo me centraré en algo mas general, no en algo tan específico de la organización de las bases de datos y sus estrategias, pero como mencioné antes, os dejo el enlace con toda la explicación por parte de los ingenieros de Netflix.

Cabe mencionar también el gran trabajo de Netflix en cuanto a BigData, ya que mediante aprendizaje automático es capaz de mostrarte las series y películas recomendadas y hasta de personalizar las miniaturas para cada persona.

### Gestión de los vídeos
Ahora pasamos a la parte sobre la gestión los vídeos por parte de Netflix. Antes de que puedas ver un vídeo en tu dispositivo favorito, Netflix tiene que pasarlo a un formato idóneo para tu dispositivo. Este proceso se conoce como transcodificación o codificación. La transcodificación es el proceso por el cual se pasa un archivo de vídeo de un formato a otro para que se pueda en diferentes plataformas y dispositivos. Netflix realiza esta tarea desde AWS utilizando 300.000 CPUs a la vez, impresionante.

Para ello, Netflix recibe un video de las productoras y estudios que ocupa varios terabytes y luego mediante un proceso de transcodificación se generan muchos archivos, cada uno para un dispositivo ideal y hasta para una calidad de conexión, todo esto teniendo en cuenta que dan soporte a 220 dispositivos, por lo tanto nos podemos hacer una idea. Como ejemplo, desde el Tech Blog nos proporcionan los siguientes datos: para The Crown, necesitaron cerca de 1200 archivos, mientras que para la segunda temporada de Stranger Things (que fue grabada en 8K) llegaron a obtener 9570 archivos diferentes entre vídeo, audio y texto, gastando 190.000h de CPU.

## ¿Cómo llegan todos esos datos a mi pantalla?
Netflix ha probado tres estrategias diferentes para reproducir vídeo en streaming: su propia pequeña red de distribución de contenidos, redes de distribución de contenidos de terceros y Open Connect.

En el caso de Netflix, la localización central donde se almacenan los vídeos es S3. Partiendo de ahí, ¿por qué es necesaria una red de distribución de contenidos? Simple, el vídeo tiene que estar lo más cerca posible del usuario y para ello se utilizan ordenadores por todo el mundo. Cuando un usuario quiere ver un vídeo, la red busca el equipo más cercano que tenga el vídeo y el streaming se produce desde ese servidor. Las principales ventajas de utilizar una red de dsitribución son velocidad y eficacia. No me imagino cuántas quejas coleccionarían si tenemos que recibir los vídeos en Europa desde EEUU. Cada localización en la que un ordenador almacena un vídeo se llama punto de presencia (PoP, Point of Presence). Estos puntos son localizaciones físicas que proporcionan acceso a Internet y que albergan servidores, routers y otros equipos.

En 2007, cuando Netflix lanzó su servicio de reproducción en streaming, tenía 36 millones de usuarios en 50 países que veían mas de mil millones de horas de vídeo cada mes, utilizando varios terabits de contenido cada segundo. Para poder cargar con todo el servicio de streaming, Netflix creó su propia red de distribución de contenidos simple en cinco localizaciones diferentes dentro de los Estados Unidos. El catálogo de vídeos de Netflix era lo suficientemente pequeño por aquel entonces como para que cada localización pudiera almacenar todo el contenido.

Así como la anterior red de distribución era demasiado pequeña, la segunda fue demasiado grande. En 2009, decidieron usar redes de distribución de contenidos de terceros.

Tiempo después, en 2011 Netflix se dio cuenta de que, debido a su tamaño, necesitaba una solución personalizada para tener una red de distribución de contenidos más eficaz. La distribución de vídeo es una de las competencias básicas de Netflix y por aquel entonces podía suponer una enorme ventaja respecto a sus competidores. Así que comenzaron a desarrollar Open Connect, la red de distribución de contenidos diseñada específicamente para ellos que se acabó lanzando en 2012.

Como cada red de distribución consta de servidores almacenando contenido y envíandolo, Netflix se encargó de fabricar sus propios sistemas de almacenamiento y servicio en cada nodo: Open Connect Appliances que normalmente están montados en armarios de servidores:

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/oca.jpeg) ![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/ocaserver.jpg)

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/openconnectdaredevil.jpg)

Existen diferentes tipos de OCAs según su propósito. Hay OCAs grandes que pueden almacenar todo el catálogo de Netflix y los hay más pequeños que solamente guardan una parte del catálogo de vídeo. Los OCAs más pequeños están llenos de vídeos todos los días, durante las horas de menos actividad, utilizando un proceso al que Netflix llama caché proactiva. Desde el punto de vista del software, los OCAs usan el sistema operativo FreeBSD y NGINX para el servidor web. Así es, cada OCA tiene un servidor web y el vídeo se transmite en streaming usando NGINX. Para logar la mejor experiencia de vídeo posible, tendrían que tener una caché de vídeo en tu casa, algo que no es posible. Pero lo que sí que pueden hacer es poner un mini-Netflix tan cerca de tu casa como puedan.



### Localización de los OCAs
Netflix distribuye sus servidores por mas de 1.000 localizaciones por todo el mundo. En el siguiente mapa se muestran dichas localizaciones:

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/mapalocalizaciones.jpg)

Otros servicios de vídeo, como YouTube o Amazon, transmiten vídeo a través de su propia red. Estas compañías han creado, literalmente, una red global para distribuir contenidos a sus usuarios, pero debido a su complejidad y coste, Netflix adoptó un enfoque completamente distinto cuando creó su red de distribución de contenidos. Netflix no explota su propia red y ha dejado de explotar sus propios centros de datos. En su lugar, los proveedores de servicios de Internet (ISPs) aceptan poner los OCAs en sus centros de datos. Los OCAs se dan de forma gratuíta a los ISPs para que los pongan en sus redes y Netflix también pone OCAs cerca de puntos de intercambio de Internet (IXPs).

Netflix tiene todo este contenido de vídeo en S3 y cuentan con todos estos servidores de vídeo por todo el mundo. Solamente falta una cosa: los vídeos. Netflix utiliza un proceso al que llama caché proactiva para copiar de forma eficaz el vídeo a los OCAs. ¿Cómo funciona? Anteriormente hablé del BigData y su relación con Netflix, pues aprovechando los perfiles que generan sobre nosotros: cada OCA es una caché de vídeo de lo que es más probable que vayas a ver.

Netflix conoce en cada sitio del mundo con un alto nivel de precisión lo que van a ver sus suscriptores y cuándo lo van a ver. ¿Recuerdas que habíamos dicho que Netflix era una compañía basada en datos?

Utilizan los datos de popularidad para predecir qué vídeos van a ser probablemente vistos mañana en cada localización. En este caso, localización significa un conglomerado de OCAs situados dentro de un ISP o de un IXP. Copian los vídeos pronosticados a uno o más OCAs en cada localización, algo que se conoce como preposicionamiento. Los vídeos se ponen en los OCAs antes de que nadie los pida. Esto proporciona un gran servicio para los usuarios porque el vídeo que quieren ver ya está cerca, preparado y disponible para streaming. Netflix explota lo que se conoce como un sistema de caché escalonada.

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/cacheEscalonada.jpg)

Los OCAs más pequeños de los que hablábamos antes están en los ISPs y IXPs y son demasiado pequeños como para contener todo el catálogo de Netflix. Sin embargo, hay otras localizaciones que sí que tienen grandes OCAs con todo el catálogo y obtienen sus vídeos desde S3.

Cada noche, en un momento de bajo uso de la red, el OCA pregunta a uno de los microservicios qué debería tener al día siguiente almacenado y se actualiza el catálogo disponible en el OCA. De esta forma nunca hay un error de caché en Open Connect. Un error de caché sería pedir un vídeo específico a un OCA y que el OCA respondiera que no lo tiene. Puesto que Netflix sabe qué vídeos tiene que tener en la caché, sabe exáctamente dónde está cada vídeo en cualquier momento. Si un OCA más pequeño no tiene el vídeo, siempre habrá un OCA más grande que lo tenga.

Pongamos un ejemplo para que se vea mejor. ¿Dónde podremos encontrar una serie muy popular como 'Narcos'? Os aseguro que estará en, practicamente, todos los OCAs. Al ser una serie muy popular, en caso de no estar en el OCA de España (por ejemplo), aunque estuviera cerca (Francia por ejemplo), si los españoles aficionados a la serie decidieran verla ese día, colapsaria la red al haber tantas peticiones a un servidor de Francia. En cambio, si lo hacen teniendo la serie en los OCAs de España, al haber varios servidores dedicados a esa labor, no se colapsarán y lo servirán sin mayor problema.

Pero, ¿por qué los ISPs estarían dispuestos a alojar OCAs? Por interés. Explico: cuándo desde casa realizamos una búsqueda en Google, teniendo Vodafone contratado (por ejemplo) esa consulta pasar por la red de Vodafone, y dado que Google no se encuentra en la misma red, esa solicitud pasará por la red de redes, Internet (que actúa uniendo redes). Y como ya comenté antes, Netflix hace unos años ocupaba más del 30% del tráfico de Internet de los EEUU. Al alojar los ISPs los OCAs en sus sedes, el tráfico de series no llegara a Internet en casi ningún momento. En el caso contrario, Netflix colapsaría tanto el Internet, que sería casi imposible acceder a él.

OpenConnect, en definitiva, es un sistema eficaz y resistente. Actúa como un archipiélago de islas a lo largo del mundo de forma que si una de ellas falla, el cliente de Netflix se da cuenta y nos redirige a otra de ellas sin que nos percatemos del error. Lo mismo sucederá en el caso de que la red esté saturada, por ejemplo. Esto lo consiguen a través de un proyecto llamado Isthmus.

## Isthmus, Active-Active y ELB
Para entender los dos primeros proyectos de Netflix, primero explicaré en que consiste en ELB en AWS, un balanceador de carga un poco particular: Elastic Load Balancing distribuye automáticamente el tráfico de aplicaciones entrantes a través de varios destinos, tales como instancias de Amazon EC2, contenedores, direcciones IP y funciones Lambda. Puede controlar la carga variable del tráfico de su aplicación en una única zona o en varias zonas de disponibilidad. Elastic Load Balancing ofrece tres tipos de balanceadores de carga que cuentan con el nivel necesario de alta disponibilidad, escalabilidad automática y seguridad para que sus aplicaciones sean tolerantes a errores.

El punto de partida del proyecto Isthmus de Netflix fue la necesidad de ofrecer streaming de calidad y sin caídas ni siquiera aumentos notables del tiempo de servicio incluso si los ELBs se caían. Decidieron pribar si conseguían mantener la latencia enviando de forma 'duplicada' los datos, mediante el ELB de AWS y mediante lo que ellos llamarron itsmos que conectaban entre si, diferentes zonas de una región de AWS mediante DNS y, en caso de fallar, tomar el mando como vía principal de transmisión de datos. En las primeras pruebas era un ingeniero, a mano, quien ejecutaba dicho cambio. Más adelante nace de su mano: Denominator, una biblioteca de código abierto para trabajar con proveedores DNS. Finalmente nació Zuul una capa de enrutamiento desarrollada por ellos mismos que mantiene el flujo de conexiones, permite un filtrado inteligente...

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/isthmusok.png)

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/isthmusfail.png)

A continuación surge el proyecto Active-Active, que buscaba conectividad sin caídas ahora entre regiones. Hablando específicamente sobre el tema podríamos pasar horas, pero resumidamente es eso lo que buscaba, unir sus regiones por otras vías para aportar redundancia entre conexiones.

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/activeactiveok.png)

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/activeactiefail.png)

## Finalmente, ¿qué sucede cuando le damos al play?
- Podemos dividir Netflix en tres partes: servidor, cliente y red de distribución.
- Todas las solicitudes de los clientes las maneja AWS.
- Todo el vídeo se transmite desde una OCA cercana a la red Open Connect.
- Existen cientos de ficheros diferentes para un mismo vídeo.
- Todos los días, a través de Open Connect, distribuye vídeos en todo el mundo, según sus pronósticos sobre lo que van a querer ver los miembros de cada zona.

![img](https://github.com/bertoig/SWAP_UGR/blob/master/Trabajo%20Final/Images/Netflixfinal.jpg)

1. Seleccionas un vídeo que quieres ver usando un cliente que se ejecuta en algún dispositivo. El cliente envía una solicitud de reproducción (play) que indica qué vídeo deseas reproducir al servicio de aplicaciones de reproducción de Netflix que se ejecuta en AWS.

2. No hemos hablado de esto, pero una gran parte de lo que sucede después de darle al play tiene que ver con las licencias porque no todos los vídeos se pueden ver en todo el mundo. Netflix debe determinar si tienes una licencia válida para ver un vídeo en particular.

3. Una vez que ha tenido en cuenta toda la información relevante, el servicio Playback Apps devuelve direcciones URL para hasta diez servidores OCA diferentes. Este es el mismo tipo de URL que normalmente utilizas en tu navegador. Netflix utiliza tu dirección IP y la información de los ISP para identificar qué servidor de OCA es el que te viene mejor.

4. El cliente selecciona de forma inteligente cuál es el mejor OCA que puede usar. Para ello comprueba la calidad de la conexión de red a cada OCA y se conectará al servidor OCA que sea más rápido y fiable. El cliente continúa realizando estas pruebas durante todo el proceso de transmisión de vídeo.

5. El cliente busca la mejor manera de recibir contenido desde el OCA.

6. El cliente se conecta al OCA y comienza a transmitir vídeo a tu dispositivo.

7. ¿Alguna vez has notado que al ver un vídeo cambia la calidad de la imagen? ¿A veces se pixela y después de un tiempo la imagen vuelve a calidad HD? Eso es porque el cliente se está adaptando a la calidad de la red. Si la calidad de la red disminuye, el cliente reduce la calidad del vídeo para que coincida. El cliente cambiará a otro OCA si la calidad disminuye demasiado.

Tras todo este trabajo, finalmente, podremos ver nuestra serie favorita en Netflix. A lo largo del trabajo, he ido explicando el funcionamiento de Netflix desde sus microservicios hasta sus servidores centrales, proyectos que han ayudado a la mejora de la conexión y hardware específico diseñado por ellos mismos. En conclusión, el trabajo y las aportaciones de Netflix al mundo van mucho mas allá de lo cultural, a lo largo de todos estos años han hecho enormes aportes a la comunidad informática del software libre y animo a cualquiera a seguir informándose en su blog, su repositorio de softare y mismamente en sus redes sociales.


# Bibliografía

- https://medium.com/netflix-techblog
- https://netflix.github.io/
- https://www.xataka.com/streaming/la-compleja-infraestructura-detras-de-netflix-que-pasa-cuando-le-das-al-play
- https://medium.com/refraction-tech-everything/how-netflix-works-the-hugely-simplified-complex-stuff-that-happens-every-time-you-hit-play-3a40c9be254b
