## CATEGORIAS DE ATAQUE

#### seguridad de la informacion
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


## INFORMATION GATHERING

usado tanto en el caso de una auditoria convencional como en el propio ataque a un objetivo determinado
- recolección de informacion (maltego, osin)

#### la trilogia
host descovery host scanning fingerprinting

#### information gathering activo
buscar activamente mediante peticiones informacion sobre un objetivo, llamadas ...

#### information gathering pasivo
conseguir informacion mediante la observacion de redes sociales, informacion publica

#### scanning pasivo
conseguir informacion mediante el analisis de propiedades de paqueteria

#### gingerprinting
conseguir informacicon del software y versiones sobre un objetivo

### google hacking/dorks
usar google o otro buscador para information gathering

#### servicios MX
estafetas de correo

#### servicios DNS
domain name service (name -> ip) inverso: (ip -> name)

#### traceroute
command to check the path of packet
its return message may be blocked

### dns cache snooping
analizar si el objetivo esta en el cache (dnsrecon)
- no-recursivo: dns solo resuelve si esta en chache
- recursivo: llama a los dns auxiliares
algunos caches no permiten la primera opcion 
solucion: dos llamadas, la primera guarda, la segunda estara en cache, analizar e

### port scanning
analizar la situacion de los puertos de la maquina
activa: mandar paquetes directamente
pasiva: observar paqueteria y sus cabecers
sistemas de inclusion, obervar servicios activos, dmz

#### dmz
zona desmilitarizada: red de zonas expuestas a internet

herramientas de analisis de herramientas: nessus

#### WAF
web application firewall: modsecurity


#### OWASP 
OWASP TOP 10 -> 10 debilidades mas recurrentes en los ultimos 4 años

OWASP TOP 10 PROACTIVE CONTROL -> categorias control a la hora de desarrollar cualquier arquiectura web

OWASP APPLIACTION SECURITY VERIFICATION STANDARD -> buenas practicas desarrollo web

#### host discovery

HOST DISCOVERY
que maquinas estan encendidas y cuales no

TCP -> 3-way handshake 
```
    |S|            |D|
     |     SYN>     |
     |    <SYN+ACK  |
     |     SYN>     |
            
            ...

     |              |
     |     FIN>     |
     |    <ACK      |
     |    <FIN      |
     |     ACK>     |
```
##### tipos de flags:
SYN
RST
ACK
FIN

PSH -> PUSH (ir procesando paquetes)
URG -> prioridad (saltar pila de lectura)
ECE -> reducir ventana a la mitad
CWR -> indicar ECE recibido
    descongestionar red
NS  -> "castigao" Nonce Sum

NMAP -> 4 estados
    open        -> <SYN+ACK
    closed      -> <RST
    filtered
    unfiltered
    open | filtered
    closed | filtered

como descubrir maquinas en red usando TCP

### TCP SCAN
OPEN:
```
    |S|            |D|
     |     SYN>     | (P80)
     |    <SYN+ACK  |
     |     SYN>     |
            ...
```

CLOSED:
```
    |S|            |D|
     |     SYN>     | (P80)
     |    <RST      |
            ...
```

DOWN:
```
    |S|            |D|
     |     SYN>     | (P80)
     |              |
            ...
```

#### STEALTH SCAN 
HALF-OPEN SCAN -> scaneo menos paqueteria

OPEN:
```
    |S|            |D|
     |     SYN>     |
     |    <SYN+ACK  |
     |     RST>     |
            
            ...
```

CLOSED:
```
    |S|            |D|
     |     SYN>     | 
     |    <RST      |
            
            ...
```

#### ACK-SCAN:

UNFILTERED:
```
    |S|            |D|
     |     SYN>     | 
     |    <RST      |
            ...
```

FILTERED:
```
    |S|            |D|
     |     SYN>     | 
     |              |
            ...
     NO RESPONSE
     ICMP ERROR
```

#### FIN-SCAN:
```
    OPEN|FILTERED
    |S|            |D|
     |     SYN>     | 
     |    <RST      |
            ...
    CLOSED
    |S|            |D|
     |     FIN>     | 
     |  <ACK|RST    |
            ...
```

#### NULL-SCAN 
sequence number set to 0
OPEN|FILTERED:
```
    |S|            |D|
     |    NULL>     | 
            ...
```

CLOSED:
```
    |S|            |D|
     |    NULL>     | 
     |  <ACK|RST    |
            ...
```

#### XMAS-SCAN:
```
    OPEN|FILTERED
    |S|            |D|
     | FIN+URG+PSH> | 
            ...
```

CLOSED:
```
    |S|            |D|
     | FIN+URG+PSH> | 
     |  <ACK|RST    |
            ...
```

#### IDLE-SCAN
pivotar sobre impresora (IPiD)

CONDICIONES:
- TENER UN ZOMBIE
- POCO TRAFICO
- IPID

OPEN:
```
    |A|            |V|            |Z| (spoof / mine)
     |   SYN+ACK>   |              |
     |  <RST+IPid   |              |
     |              |  <SYN+ACK    |
     |              | RST+IPid+1>  |
     |   SYN+ACK>   |              |
     | <RST+IPid+2  |              |
```

CLOSED|FILTERED:
```
    |A|            |V|            |Z| (spoof / mine)
     |   SYN+ACK>   |              |
     |  <RST+IPid   |              |
     |              |    <RST      |
     |   SYN+ACK>   |              |
     | <RST+IPid+1  |              |
```

|IANA| -> Internet Assigned Numbers Authority


### HOST DISCOVERY UDP -> 

    |C|            [S]
     |  UDP+PORT>   |   (53, DNS)
     |  <UDP+DATA   |

    |C|            [S]
     |  UDP+PORT>   |   (53, DNS)
     | <ICPMunreach |

HOST DESCOVERY ICMP (trafico pi)
cabeceras especificas
    ICMP echo request   (ping)
        |
    ICMP echo reply

    si no se devuelve: apagada, filtrada o cerrada

    ICMP timestamp // ICMP mask address

    PING SWEEP (barrido ping) (NMAP)
        1) ICMP echo request -> ICMP echp reply
        2) TCP SYN (443)     -> ACK or RST    
        3) TCP ACK (80)      -> RST
        4) ICMP timestamp    -> ICMP timestamp reply

        es detectable -> bloquear trafico ICMP
        reconocible

        nmap different if sudo or not

        ¬SUDO -> ARP table (--send-ip disbles ARP)
        SUDO  -> connect() 

    si el tiempo usado es demasiado grande, puede ser detectado por IPS/IDS

    para evitar esto,nmap suele usar TIMEOUTS
        CALLS/sec
        THREADS
            MONO    MULTI   ALL
            -T0     -T3     -T5
            -T1     -T4
            -T2
        IDS SLOW READING -> 
        1) fragmentation // MTU leer cachitos 
        2) --badsum -> IDS ignora
        3) --data-length -> añadir 0s
        4) direccion ip//MAC//interface zombie// ilegitimas
            interfaces suelen no estar protegidas
        5) decoy(señuelo) -D
            (usar las 50 para conectarse a las 48)
        6) --randomized-hosts 
        7) TTL (timetolive) (DDos(infinite) 1(answer))

    NSE: Nmap Scripting Engine
        1) broadcast
        2) brute
        3) discovery
        dos
        exploit
        malware
        version
        vulnerability
        intrusive
            tirar abajo el IPS

        -D & -F call NSE

        80 -> [apache, WGINX, API...]


### fingerprinting


1st GEN: reconocimiento ascii -> netcat `nc` (conexion directa con un puerto)
         HEAD, GET, POST, OPTIONS

more modern:
WFETCH
PAROSPROXY

FIDDLER
BURPROXY

tampering -> modificacion de cabeceras/info  que se realiza con un servicio

HTTPS:// user:pass  @ udc.es:80/ indice / entrada.app
protocol credentials server port ruta_recurso

parameter tampering

tokens: cookies, session, url -> if url, can be obtained from 

2nd GEN:    
    ANALISIS TCP/IP -> brute force (huella tcp/ip)
    well-known ports: [1-1023] permisos especiales
    [1024-49151]: registered poorts 
    [492152-65535]: dynamic ports (auxiliares)
    FTP -> file transfer protocol (auth)

3rd ICMP TRAFIC

    ICMP ECHO REQUEST
    ICMP  ECHO REPLY
    PORT UNREACHABLE

    `SING`
    `XPROBE2`

4th ANALISIS APLICACIONES (tcp-ip) (sesion, representacion y applicacion OSI)
    protocolo apps 
    UDP -> FTP, HHTP, SMTP, DNS
    TCP -> DNS

nmap -O [IP]
-- osscan-guess --fuzzy <- aproximacion del SO 

OFUSCACIONES :  

TTL ->  
    Linux   ->  60,64
    MacOs   ->  60,64
    Windows ->  32,128

modify kernel variable (TTL)
    /proc/sys/...
    /proc/sys/net/ipv4/ip_default_ttl

TCP WINDOW  
    Linux   ->  5720,65535(freebds)
    MacOs   ->  5720
    Windows ->  65535,8192

petition window size:
    /proc/sys/net/ipv4/tcp_window_scaling

instead of modifying on the machne proper, we can modify the function of the firewall

osfuscate

honeybot -> señuelo () 
honeynet -> * buscar

lynx <- navegador terminal


apache variables:
SERVERTOKEN: {prod,major,minor,min,os,full} <- os default
    full: preloaded modules
    mod, evasive <- otro modulo()

    DDOS

SERVERSIGNATURE {on, off} <_ off = PROD (only service, no version, no modules)

whatweb [dominio] 
consigue info sobre versiones 

lynis -> info sobre seguridad para tu equipo
lynis update info
lynis audit system

lynix 

lynis show details [error code]

cuantas particiones necesito

