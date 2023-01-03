### CATEGORIAS DE ATAQUE

### seguridad de la informacion
medidas preventivas y reactivas del hombre, organizaciones y sistemas tecnologicos que permitan resgardar y proteger la informacion manteniendo la Confidencialidad Integridad y Disponibilidad

#### seguridad informatica
nombre generico que se da al grupo de herramientas diseñadas para proteger los datos y evitar la intrusión de los hackers
es un subset de la seguridad de la informacion

### vulnerabilidad
debilidad (hardware/software...) de un activo o control que puede ser explotada por una amenaza

el CVE es el estandar de nomenlatura, listado de vulnerabilidades con descripción e id, es de uso publico y no tiene informacion sobre riesgo, impacto informacion sobre la correccion o detalles tecnicos
de todo ello se encarga la National Vulnerability Database
tambien existe el CWD, que coje la CVE y clasifica las amenazas

vulnerabilidad zero-day: se encuentran en el momento y no se han podido arreglar

ejemplos: meltdown, spectre o krack attacks

#### amenaza 
posibilidad de violacion de la seguridad que existe cuando se da una cirtcunstancia, capacidad, accion o evento que pudiera romper la seguridad y causar perjuicio

ejemplos: time bomb, bots, virus, worms, trojan...

#### ataque
accion que comproometa la seguridad de la informacion de una organizacion

ejemlos: DoS, siniffin, dns poisoning, brute force...

flujo normal de la informacion:
A ----> B

interrupción:       modificación:
A --|  B            A       B
                    |       ^
                    |       |
                    --> C ---

interceptación:     fabricacion
A ----> B           A       B
    |                       ^
    v                       |
    C                   C ---

es muy comun este flujo en cualquier contexto informatico (cualquiera de las capas OSI)

#### IDS
sistema de deteccion de intrusiones
- a nivel de host (servidor)
- en la red
- como firewall

#### IPS
sistema de prevencion de intrusiones
pattern matching con una intrusion y una accion

#### sensores
red de agentes en una conexion cliente-servidor (distribuir ips)

#### hijacking 
adueñarse o robar algo por parte de un atacante

#### DDOS
tirar un servicio hasta que pete
- filtrar? -> si empresa utiliza un proxy? distribuido?
- controlar el ancho de banda
    en linux: `tcpkill`, en firewalls hashlimit

#### ataques contra la integridad
hash (firma digital y cosas)
