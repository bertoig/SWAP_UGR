# Sistemas de ficheros en red más usados actualmente
A lo largo de este documento comentaré un poco cuales son los sistemas de ficheros en red mas utilizados en la actualidad así como sus ventajas y desventajas.

## AppleShare
Comenzaré por uno de los más famosos de la actualidad. El sistema de ficheros AppleShare como su nombre indica fue creado por el gigante de la tecnología Apple.

Cuando de Apple se trata, nos encontramos siempre el mismo problema: un uso demasiado privativo. Esta vez hasta se superan a ellos mismos en este aspecto desarrollando sus propios protocolos para el uso de este sistema de ficheros.

La gran ventaja de esta herramienta de Apple siempre será la fiabilidad y las prestaciones que nos aporta siempre Apple así como, en el ámbito de los desarrolladores, el uso de Unix.

## Andrew File System
Este un sistema de archivos distribuido a través de la red que fue desarrollado como parte del proyecto Andrew por la Universidad Carnegie Mellon.​ Su nombre proviene de Andrew Carnegie y Andrew Mellon. Es utilizado fundamentalmente en entornos de computación distribuida.

AFS tiene varios beneficios sobre los sistemas de archivos en red tradicionales, en particular en áreas de seguridad y escalabilidad. Es frecuente que los despliegues de AFS en empresas lleguen a tener más de 25.000 clientes.​ Usa Kerberos como mecanismo de autenticación, e implementa listas de control de acceso en directorios para usuarios y grupos. Cada cliente mantiene una caché en el sistema de archivos local para aumentar la velocidad de acceso a los archivos. Esto también permite el acceso limitado al sistema de archivos en el caso de caída del servidor o un fallo de red.

Una característica importante de AFS es el volumen, un árbol de archivos y subdirectorios. Los volúmenes los crean los administradores y los enlazan con una ruta específica en una celda AFS. Una vez ha sido creado, los usuarios del sistema de archivos pueden crear directorios y archivos de manera normal sin tener en cuenta donde se encuentra físicamente el volumen. Un volumen puede tener una cuota asignada para limitar la cantidad de espacio consumido. Según las necesidades, los administradores de AFS pueden mover ese volumen a otro servidor y otra localización en disco sin la necesidad de notificar a los usuarios de dicho cambio; de hecho esta operación puede realizarse mientras se están usando los archivos dentro del volumen.

Los volúmenes pueden ser replicados para copias de respaldo de sólo lectura. Cuando se accede a archivos en un volumen de sólo lectura, un sistema cliente obtendrá datos de una copia de sólo lectura particular. Si en algún punto esa copia deja de estar disponible, el cliente buscará otra de las copias restantes. De nuevo, los usuarios de esos datos se despreocupan sobre la localización física de esta copia; los administradores pueden crear y recolocar tales copias según las necesidades. La suite de comandos AFS garantiza que todos los volúmenes de sólo lectura contienen copias iguales del volumen original de lectura-escritura en el momento que se creó la copia de sólo lectura.

El espacio de nombres de archivos en una estación de trabajo Andrew es particionada en dos espacios: el espacio de nombre compartido y el local. El espacio de nombres compartido es idéntico en todas las estaciones. El espacio local es único para cada estación. Sólo contiene archivos temporales necesarios para la inicialización de la estación y enlaces simbólicos a los archivos que se encuentran en el espacio de nombres compartido.

## Network File System
Introducido  por  Sun  Microsystems  en  1985,  fue  desarrollado  originalmente para  UNIX.  Se  concibió  como  sistema  abierto,  lo  que  le  ha  permitido  ser adoptado  por  todas  las  familias  UNIX  y  por  otros  sistemas  operativos  (VMS, Windows),   convirtiéndose   en   un   estándar   de   facto   en   LANs.NFS   ha evolucionado mucho y la Versión 4 actual poco tiene que ver con las anteriores, ya  que  incluye  estado  y  la  posibilidad  de  implementación  en  WAN.

Los  servidores exportan  directorios.  Para  hacer  exportable  un  directorio  se incluye el path en un determinado fichero de configuración. Los clientes montanlos   directorios   exportados,   y   estos   se   ven   en   el   cliente   completamente integrados  en  el  sistema  de  ficheros.  El  montaje  se  ejecuta  en  el  booting  del sistema  operativo,  o  por  demanda  cuando  se  abre  un  fichero  mediante  un servicio  adicional  de  NFS,  elautomounter.  Las  operaciones  sobre  ficheros  y  las peticiones de montar son atendidas por sendos procesos daemon en el servidor (nfsd y mountd respectivamente).

El mantenimiento de la consistencia UNIX resulta problemático. La disminución  del  periodo  de  validación  para  mejorar  la  consistencia  produce una sobrecarga por la gran cantidad de operaciones getattr que se realizan. En  principio,  el  montaje  de  sistemas  de  ficheros  remotos  no  era  transparente (hay  que  identificar  al  servidor).  El automounter,  una  utilidad  que  permite  el montaje dinámico de sistemas de ficheros por demanda, mejora este aspecto.Debido  a  la  falta  de  estado,  bloquear el acceso a ficheros remotos requiere un mecanismo de exclusión mutua independiente. En UNIX se utiliza un servidor específico, lockd.


##Server Message Block
Server Message Block (SMB)​ es un protocolo de red que permite compartir archivos, impresoras, etcétera, entre nodos de una red de computadoras que usan el sistema operativo Microsoft Windows. Es utilizado principalmente en computadoras con sistemas operativos: Microsoft Windows y DOS.

En 1998, Microsoft renombró SMB a Common Internet File System (CIFS) y añadió más características, que incluyen: soporte para enlaces simbólicos, enlaces duros (hard links), y mayores tamaños de archivo. Hay características en la implementación SMB de Microsoft que no son parte del protocolo SMB original.

Los servicios de impresión y el SMB para compartir archivos se transformaron en el pilar de las redes de Microsoft. Con la presentación de la serie Windows 2000, Microsoft cambió la estructura de incremento continuo para el uso del SMB. En versiones anteriores de los productos de Microsoft, los servicios de SMB utilizaron un protocolo que no es TCP/IP para implementar la resolución de nombres de dominio. Comenzando con Windows 2000, todos los productos subsiguientes de Microsoft utilizan denominación Domain Name System (DNS). Esto permite a los protocolos TCP/IP admitir directamente el compartir recursos SMB.

Uno de sus inconvenientes es la vulnerablidad: el exploit de una vulnerabilidad supuestamente creado por la NSA llamado EternalBlue fue utilizado en el ataque mundial de ransomware con WannaCry del 12 mayo de 2017.

##Conclusión
Como vemos existen numerosos sistemas de ficheros en red, y muchos más que quedarían por nombrar. En este trabajo realicé un pequeño resumen de sus historias, usos y vulnerabilidades. Como vemos, en todos ellos encontramos una serie de numerosas ventajas y otras tantas desventajas. Tras este análisis podremos concluir que la elección de un sistema de archivos en red es algo muy importante y que se basará en las condiciones iniciales de uso que tengamos.
