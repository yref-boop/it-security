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


## ocultacion y privacidad

NAT -> ip publica en vez de privada (ocultacion)
    estático:
    dinamico:
    pat: nat para puertos

PROXY -> intermediario en comunicasion
    HTTP/HTTP/S (apache)
    ANONIMO (ocultacion)
    TRANSPARENTE (nada)
    ELITE
    NOISY -> X_FOR... habilitado pero con ips no reales/ escogidas a consciencia
    SOCKS (SOCKET SECURE) -> 
        SOCKS4 -> TCP & insecure/no cifrada
        SOCKS5 -> TCP/UDP & seguridad/cifrado
    REVERSE ???
    CACHE -> 
        SQUID CACHE     -> X_FORWARDED_FOR //HEADERS (habilitado = transparente)
        APACHE HTTP/S   -> RUTAS

ocultacion siempre
privacidad segun el cifrado utilizado

(pre) shared key
certificados
user-password


#### crackeo wifi
- WEP
- WPS
- LUPA
- WPA
- WPA2
- WPA3

4 way-handshake
```
     CLIENTE                     ROUTER
 passw  |                           | password (Pre-Shared Key) / (EAP)
SSID    |                           | SSID
PMK     |                           | PMK
        |                           |
        |      <1.ANONCE            | ANONCE
PTK MIC |                           |
SNONCE  |      SNONCE+(MIC)1.       |
        |                           | 
        |                           | PTK, (GTK)
        |   <3.INSTALTION+MIC+(GTK) |
        |   4.INSTALATION+MIC>      |
```

`PTK` is used as the cypher

`PMK` : pairwise master key = SSUD + hash(PSK)
`ANONCE` : authenticator only use number
`PTK` :  PMK + MACc + MACr + ANONCE + SNONCE
`SNONCE` : supplicant only use number
`MIC`: message integrity code

ATAQUES
    
PMK = SSID + *hash(PSK)
PTK =  PMK + *MACc + *MACr + ANONCE + SNONCE

aircrack: modo monitor: escuchar todo protocolo wifi activo en 80m
- airmon-ng -> modo monitor
- airodump-ng ->
- aireplay-ng -> trafico especifico
    por ejemplo desconectar a un cliente -> reconexion automatica
- aircrack-ng -> conseguir PSK (diccionarios)
    - generadores: crunch (regex), the harvester (recopila posibles candidatos a diccionario)
    - crackers: john the ripper, cain & abel, medusa, hydra, hashcat
        - hydra, haschat best -> distributed cracking, multithreading, regex
- airbase-ng -> |evil twin|
        
idea diccionarios: guarda posibilidades y paso al otro lado para comprobar si funcionan -> user+(PASS)hash

#### RAINBOW TABLE
en vez de guardar candidatos y mandar a que se ejecuten y comprueben, guardar unicamente el resultado de aplicar el hash a cada valor
en el momento en el que unico encaje, ya se sabe directamente el resultado
 
problems on 3. -> con esto se esta atacando a la ocultacion, hasta el paso 3
krack attack > key re-installation attack

no se tuvo en cuanta en que anonce y snonce son valores incrementales, si a alguien se le ocurria reenviar el tercer paquete, anonce y snonce se reinician -> todo el descifrado de informacion se deshace-> se pone todo a 0s --> el unico valor desconocido es el PSK 
esto permite que se pierda la privacidad en la conexion wifi -> todo es accesible como texto plano


#### WPA3
2008 presentado, 2018 finalmente implementado, 2019 crakeado 
    (vulneabilidad: dragon blood)
    herramientas:
        dragondrain -> DOS => "solucion"forzar a pasar a WPA2
        dragontime -> jode el tiempo
        dragonforce -> crackeo contraseñas
        dragonslayer -> 
protocolo dragonfly (not 3 way handshake)

ROGUE ACCESS POINT ->  
    tools -> Social Egineering Toolkit, FLUXION


#### privacidad

#### disco
rm -> srm -> shred || scrub

srm -> sobreescritura
shred -> n sobreescrituras
scrub -> n sobreescrituras a nivel fichero y carpeta (incluyendo nombres)

SFILL -> ocultar burbujas sin datos

NIVEL SWAP ->

NIVEL RAM -> mimikatz (desofuscar informacion en la ram)

history ->  lista de comandos 


#### TOR The Onion Router
user -> onion proxy -(min 3 machines)-> Directory Server (knows all internal machines) 
pacman
     
funcionamiento:

    all using tls() 
    1st machine (entry relay)   <- estático (~60días)
    2nd machine (middle relay)  <- dinámico
    3rd machine (exit relay)    <- dinámico 

encapsulado paquetes:
- maquina original:
(ip_1+key_1(ip_2+key_2(ip_3+key_3(paquete original))))
- maquina 1
ip_2+key_2(ip_3+key_3(paquete original))
- maquina 2
ip_3+key_3(paquete original)
- maquina 3
paquete original

TAILS       -> sistema operativo completo que pasa por la red tor de forma nativa
TOR STEAM   -> libreria de python ""
TOR SOCKS   -> socks que solo pasa ""
TORTILLA    -> implementacion de socks para maquinas virtuales

internet (10%)

#### deep web
web no indexada (uris, servicios)

#### dark web
(1%) deep + autenticacion 

#### dark net
soporte a la dark web

.onion -> accesibles desde tor o 

sha.1(url).onion
"".i2p
.
.
.


I2P 
freenet
zeronet

#### inproxy
(solo se puede acceder a elementos dentro de la propia web) vpn cerrada [I2P]
##### outproxy
(se puede acceder a redes externas) [TOR]

el navegador es el mayor peligro para la privacidad
- adblock
- blockscript
- blocksite
- no-miner (ded)
- https everywhere
- cookie editor

busquedas privadas      -> 3º no sabe mis busquedas
bloquear rastreadores   
asegura encriptacion entre sitios

#### port tunneling 
incorporar servicio o protocolo dentro de otro diferente
se puede saltar firewalls
ssh tunneling ejemplo mas comun
puede tener 3 vertientes
- local: atravesar un firefox metiendo un protocolo dentro de otro (guardar un salto en uno de mis puertos)
- remoto: local a la inversa()
    ```
        /etc/sshd_config
            AllowTcpForwarding = "yes"
            GATEWAYPORTS = "yes"
    ```
- dinamico: me conecto a un ssh en el exterior que se cominica con el resto aqui tengo un proxy socks

#### port knocking 
no tener abierto todo el rato todos los puertos : definir cerraduras para ciertos puertos

#### sniffers
ademas de obtener ifo de forma pasiva, tmb se pueden usar para:
- deteccion anomalias en la red (tráfico, cabeceras)
- protocolos (si ICMP esta dehabilitado...)
    ```
    IDSs: 
        - SNORT:    sniffer tocho
        - SURICATA: sniffer tocho
    ```


-------------------------------------------------------------------------------
INTERCEPTACION
-------------------------------------------------------------------------------

#### sniffing
obtener paqueteria de maquinas ajenas
ademas se puede usar para:
- deteccion anomalias en la red (tráfico, cabeceras)
- protocolos (si ICMP esta dehabilitado...)
ejemplos: SNORT & SURICATA

#### Virtual LAN
permite estrablecer tantas redes logicas como se quiera dentro de una red física
- MAC VLAN:
- IP VLAN:
- 802.1q (TAGGING VLAN) (dot1q): añade un tag a una trama para saber a que vlan pertenece
default tag: VLAN1

#### conceptos de red            
ROUTER (capa 3, IP): 
SWITCH (capa 2, MAC): rediccionamiento (distribuye paquetes exclusivamente a su destinatario)
    se suelen usar para conseguir redundancia
dot1q permite cambiar el modo de funcionamiento de las bocas del switch
- access: comunicacion horizontal
- trunk: comunicacion vertical (utiliza Virtual Trunking Protocol o DTP)
    - DTP -> CISCO
- dynamic: redes LAN que aparecen y desaparecen en el tiempo

HUB (capa 2, MAC): amplificador (distribuye paquetes por toda la red conectada al hub)


PROBLEMAS:
SWITCH SPOOFING:
DOUBLE TAGGING: VLAN20(VLAN10(PACKAGE))


#### spanning tree protocol
decision de enrutamiento para evitar que un paquete acceda a internet sin quedarse dando vueltas o teniendo conflictos)

los switches utilizan un tipo de paqueteria especial para el STP (Bridge Protocol Data Units)
- 12 campos (BRIDGE ID, ROOT ID, ROOT PATH COST)
y un algoritmo de decision llamado Spanning Tree Algorithm
- establecer switch predominante -> todos contra todos mandando paquetes BPDU -> bridge = root id = yo, root path -> cuanto me cuesta llegar a sitio determinado
```
BRIDGE ID= [4bits] + [12bits] + [48bits]
            priority  VLANID      MAC
```
el root id se va cambiando con el root de valor mas bajo (más importante)


#### redundancia
se suelen utilizar switches para conseguirla
tmb se suelen conectar entre si

#### STP
cuando hay redundancia fisica, se intenta que no haya redundancia logica (desbloquear caminos de forma logica)
- no se actualiza el resultado hasta que se reconfigure la topologia (es estatico)
- para que sea dinamico existe el Rapid Spanning Tree Protocol


PELIGRO: 
- alguien se podria convertir en el root bridge poniendo valores mas bajos
- se inundan todos los canalaes al hacer una BPDU-TCN 
- PDU-TopologyChangeNotification <-> se ha cambiado la topologia, se tiene que volver a ejecutar el algoritmo 

(entiendase que esto es referido a la comunicacion vertical)


SOLUCIONES:
- ROOT BRIDGE WAR -> en caso de que existan conexiones estaticas nuevas, no se permite comunicacion  
- BPDU GUARD -> especificar puertos que admiten tramas BPDU de entrada
- BPDU FILTER -> especificar puertos que admiten tramas BPDU  de salida

YERSENIA -> STP VTP DTP 802.1q DHCP (identificar y manipular)


#### MAC FLOODING
llenar tabla de descubrimiento de MACs para que no pueda guardar mas
puede causar 2 cosas:
- switch inhabilitado
- hub 

lo mismo puede pasar con los routers si tienen activada la directiva ICMP REDIRECT (se usa para indicar que existe una ruta mejor a la actual para transmitir paquetes)


#### PORT STEALING
los switches utilizan una tabla para los macs (CAM TABLE)

- MACS DUPLICADAS: -> los paquetes llegan a mas de un sitio
no suele tener defensas: han aparecido agunas 
- PORT SECURITY: 1 a 1 puerto/mac
- PORT MIRRORING/SPAM: analisis de trafico
- PORT BONDING/TRUNKING: 
    - aumento ancho de banda
    - aumento tolerancia a fallos
    - balanceo de carga 
    - tipos de bonding:
        - modo 0: round robin
        - modo 1: backup
        - modo 2: balance XOR
        - modo 3: broadcast 
        - modo 4: dynamic link aggregation
        - modo 5: transmission load
        - modo 6: active load

#### DHCP SNOOPING
si no conoce al nuevo DHCP SERVER, no permite (DHCP OFFER, DHCP ACK)

dentro de la maquina atacada la cmabiamos la mac del router por la nuestra
ARPON:
    - SARPI -> no dhcp
    - DARPI |  si dhcp (lista dinamica con 2 valores importantes: TIMEOUT + CACHE)
    - HARPI |  (2 listas simultaneas)

#### NDP spoofing 
<-> ARP pa 6
- PARASITE6

DNS SPOOFING
DNS CACHE POISONING

soluciones: 
- DNS SEC (implementacion que asegura integridad & autenticidad de la informacion)
    8.8.8.8 p.ej.
- Dns Over Tls -> pasar DNS por tcp


#### interceptar informacion en una red wifi: 
EVILGRADE: framework que permite (usando DNS POISONING) suplantar actualizacion de DNS de cualquier maquina
`/etc/apt/sources.list`

#### SITE-JACKING -> suplantar / modificar cookies

HTML<4 todas las cookies eran texto plano
>=5 -> web storage
- session storage: cookie para el site actual
- local storage: todas mis cookies

#### SSL-STREAM
MITM entre cliente y servidor, abriendo una conexion https alservidor y una http entre el cliente
ademas se busca que el cliente crea que su conexion conmigo sea segura

```
    cliente -----|----- servidor
                 |
                 |
          HTTPS> | HTTPS>
          HTTP<  | <HTTPS
                 |
                 |
                MIM
             SSL STRIP
```

#### Security Information and Event Management (SIEM)
sistema de recuperacion de informacion de amenazas 
prelude()


#### HTTP Strict Transport Security
asegurarse que el trafico en siempre va a ser securizado (siempre manda https)
se aporta un token de 1 año de caducidad()
```
   cliente      browser       server
        HTTP >      |    HTTPS>
                    |    < security
                    |    HTTPS>
       <HTTPS       |    <HTTPS
```

#### recomendaciones
ip-forwarding -> solo routers
ICMP-redirects... -> no aceptar


-------------------------------------------------------------------------------
FIREWALLS
-------------------------------------------------------------------------------
intermediario que actua en funcion del trafico que recibe
usos:
- cortafuegos
- analisis de trafico
- nat (paso de ips privadas a publicas)
- marcado/tagging de paquetes 

funcionamiento:
```
PACKET                                                               
INCOME  PREROUTE    for me?   no       FORDWARD    POST ROUTING         OUTPUT
--------->|  |----< decision >---------->|  |--------->|  |----------> PACKET
                        |                               ^
                        | yes                           |
                        v             LOCAL             |
                      INPUT---------> PROCESS ----->  OUTPUT
                        |                            
                        X
```

para poder hacer este camino el firewall tiene:
- reglas: "operadores" de las ip tables
    - iptables 
        - -t -> [table: filter|mangle|] tabla por defecto => filter
        - [A(append)|D(delete)|I(input[position])|-L(list)|-F(borrar)|-P(policy)] 
        - INPUT -> [chain: ]
        - acciones:
            - -p -> protocol
            - -s -> source IP
            - -d -> destinattion IP
            - -sport -> source port
            - -dport -> destination port
            - -i -> interface
        - -j [ACCEPT|DROP|REJECT|log(no finalista)] -> que hacer

- cadenas: agrupaciones de reglas asociadas a un punto de la comunicacion 
  (toman decisiones en cada momento): estados del firewall
    - input
    - output
    - forward
    - pre-routing
    - post-routing
    cada cadena puede tener 2 tipos de politicas:
    - permisiva: por defecto acepta todo
    - restrictiva: por defecto dropea todo
    en cada una de las cadenas se configuraran las reglas opuestas a su politica

- tablas: agrupaciones estrategicas de las cadenas decide que hacer con un paquete sgun su naturaleza
    - filter (input output forward)
    - nat (prerouting forward postrouting)
    - mangle (todas) [se encarga del tagging]
    - raw (prerouting output) [en desuso]

pasar de que te bloqueen en output
- usar ips aun en uso (legacy ips)
- usar vpns
- proxy
- TOR (.onion & normales)
- tunneling

las iptables generalmente son volatiles, para configurar de forma permanente una vez configurada, primero se:
`iptables-save [OUTPUT_FILE]`
`iptables-restore [OUTPUT_FILE]`

in order to make it permanent, its recommended to create a service that loads on startup:
wanted by: networking
after: networking


existen 2 tipos de 
- stateless: un paquete es leido por las reglas independientemente de lo ocurrido con anterioridad
- stateful: los paquetes mandados con anterioridad influyen en el tratado del actual

modulos:
- connlimit: restringir las conexiones
    - --commlimit-upto N
    - --commlimit-above  N
    - --commlimit-mask prefix-longlt
    - --commlimit-saddr
    - --commlimit-daddr
- conntrack: controlar las conexiones
    - --conntrack -cstate [NEW|INVALID|RELATED|ESTABLISHED]


-------------------------------------------------------------------------------
## CERTIFICATION
-------------------------------------------------------------------------------

cifrado:
- simetrico: misma clave para encriptar y desencriptar
- asimetrico: claves distintas
    - pública: cifra
    - privada: descifra
    ejemplo0: cifrado con clave publica en destino:
        [texto](aplico publica del objetivo)---------------------> servidor (solo el objetivo tiene la privada que descifra)[texto]
    ejemplo1: autenticidad (no repudio)
        [texto](aplico privada del objetivo) + [TEXTO PLANO] ---------------------> servidor (puede determinarr si la privada que tengo es la correspondiente a la publica dedl servidor)
    ejemplo2: 
        ([texto](calculo md5)privada)+ [TEXTO PLANO] ---------------------> servidor (comprueba integridad, no repudio y atenticidad de los datos) => firma digital

criptografía: ciencia encargada de los procesos de cifrado
- clásica:
    - sustitución
        - monoalfabeto
        - polialfabeto
    - trasposición -> 
        silex : tubito
        - linea del mensaje
        - linea de codificación
- moderna:
    - cifradores de bloques (discreto) [TCP]: 
        1. se manda texto plano 
        2. encriptador (depende de una clave k)
        3. se devuelve texto encriptado
        ejemplo: DES/AES-256

    - cifradores de flujo (continuo) [UDP]:
        1. flujo de datos
        2. se va introduciendo el dato en el encriptador
        3. se devuelve dato encriptado
        ejemplo: RC4

ssh: por defecto utiliza 
- diffie-hellman ->
$B = g^b mod p$
    - g & p públicos
    - $1<g<p$
    - raiz primitiva: (g^{p-1}=1 mod p)

ejemplo funcionamiento:
|A|                 |B|
g,p                 g,p             publicos
a=randr()           b=randr()       privados
x=G^a mod p         x=G^b mod p     publicos
S=B^a mod p         S=A^b mod p     clave privada generada (anbos lados pueden cifrar)

problema: como hay cambio de informacion, un MITM puede reconstruir la clave


- rsa -> modelo de cifrado moderno de bloques 1024bits
    p & q primos grandes
    n = p * q
    \Phi (n) = (p-1)(q-1)
    1<e<\Phi (n), mcd(e,\Phi (n))=1
    e.d = 1 mod \Phi (n)

3 lagunas: 
- 11 -> 11 (eleccion de numeros no cifrables)
    dependen del valor de n (p*q) -> 0, 1, n-1 & otros 6
- (paradoja del cumpleaños)
- cifrado cíclico


firma digital:
1. clave_privada(documento) + clave publica [asegurar identidad]
2. clave_privada(hash(documento)) + doc clave pública

conseguimos con esto: autenticacion(clave_privada), integridad de los datos(hash), no repudio

certificado digital: documento publico firmado digitalmente por entidad certificadora que recoge identidad de "algo" o "alguien"

en españa entidad certificadora: FNMT
internacionalmente: TRUST, VERISIGN, GEOTRUST

los navegadores tienen la clave publica de las entidades certificadoras descargadas por defecto

una maquina es entidad certificadora, otra servidor apache
servidor pasa su clave publica para certificar
e.c. firma con su clave publica y pasa firma a servidor apache
se pone la clave pulica firmada como publica del apache

carpeta copia claves publicas, comando detecta e instala
host normal: usar chrome certificaciones instalar entidad certificadora añadir


CRL certificate revocation list: entidad certificadora revoca los certificados existentes en esta lista

