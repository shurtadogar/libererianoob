# Network Mapper

## INTRODUCCIÓN

**NMAP** o **Network Mapper**, es una herramienta de ciberseguridad de código abierto que se utiliza para escanear redes y evaluar la seguridad de los sistemas. Su objetivo principal es ayudar a los administradores de redes y profesionales de seguridad a descubrir hosts en una red, identificar los servicios y puertos abiertos en esos hosts, y detectar posibles vulnerabilidades en los sistemas objetivo.

Principalmente esta herramienta es utilizada para 3 cosas:

* Auditorias de seguridad
* Pruebas rutinarias de escaneo de redes
* Recolección de información para futuros ataques

### USOS DE NMAP

Esta herramienta se utiliza para diversas finalidades en el ámbito de la seguridad informática. Algunos de los usos más comunes son:&#x20;

1. Descubrimiento de hosts: puede identificar los sistemas activos en una red específica, lo que permite conocer qué dispositivos están conectados y obtener una visión general de la estructura de la red.
2. Escaneo de puertos: permite verificar el estado de los puertos en un host objetivo, determinando si están abiertos, cerrados o filtrados. Esto es útil para identificar los servicios disponibles en un sistema y evaluar la exposición a posibles ataques.
3. Detección de servicios y versiones: puede identificar los servicios en ejecución en los puertos abiertos, así como las versiones específicas de esos servicios. Esta información es valiosa para detectar posibles vulnerabilidades y configuraciones inseguras.
4. Análisis de seguridad de red: se utiliza para evaluar la seguridad de una red, identificando posibles puntos débiles, configuraciones incorrectas o sistemas vulnerables. Esto permite a los administradores de red tomar medidas proactivas para fortalecer la seguridad de la red.

### CARACTERÍSTICAS PRINCIPALES&#x20;

La herramienta ofrece una amplia gama de características y funcionalidades que son valiosas para las tareas de escaneo de red y evaluación de seguridad. Algunas de sus características clave son:

1. Escaneo de puertos: puede realizar escaneos de puertos en hosts objetivo para identificar qué puertos están abiertos, cerrados o filtrados.
2. Detección de servicios y versiones: La herramienta puede identificar los servicios que se están ejecutando en los puertos abiertos, así como las versiones de esos servicios.
3. Escaneo de descubrimiento de hosts: puede realizar escaneos de descubrimiento para encontrar hosts activos en una red determinada.
4. Escaneo de vulnerabilidades: tiene la capacidad de realizar escaneos específicos para detectar vulnerabilidades conocidas en los sistemas objetivo.
5. Flexibilidad y personalización: ofrece numerosas opciones y ajustes que permiten a los usuarios adaptar y personalizar sus escaneos según sus necesidades. Esto incluye técnicas de escaneo, tiempos de espera, puertos personalizados y scripts NSE para tareas adicionales.
6. Compatibilidad multiplataforma: es compatible con varias plataformas, incluyendo Windows, macOS y Linux, lo que permite su uso en diferentes sistemas operativos.

La herramienta es altamente versátil y tiene muchas más funcionalidades que pueden ser exploradas para adaptarse a diversos casos de uso en seguridad de redes.

***

## ESCANEO DE PUERTOS

### ESCANEO TCP SEMI ABIERTO

El escaneo **TCP SYN**, también conocido como **escaneo semiabierto**, es una técnica utilizada para descubrir qué puertos están abiertos en un sistema objetivo en una red.&#x20;

A diferencia de otros métodos de escaneo, no se completa el proceso de establecimiento de una conexión TCP. En lugar de eso, se envía un segmento SYN al servidor y se espera la respuesta SYN-ACK. Si se recibe un SYN-ACK, se interpreta que el puerto está abierto, pero no se envía el segmento ACK para completar el proceso de conexión.&#x20;

El objetivo es evitar dejar registros de actividad en la máquina objetivo. Sin embargo, algunos sistemas de seguridad pueden detectar este tipo de escaneo.

```
nmap -sS <ip_address>
```

### ESCANEO TCP CONNECT

Este escaneo es muy similar al anterior pero en este método se establece una conexión completa con el servidor objetivo, lo que aumenta el riesgo de ser detectado y registrado en los registros del servidor.&#x20;

```
nmap -sT <ip_address>
```

### ESCANEO FIN NULL Y XMAS

El **escaneo FIN** aprovecha el hecho de que algunos cortafuegos están configurados para interceptar paquetes **SYN**. En este escaneo, NMAP envía paquetes con el **flag FIN** activado, lo que puede permitir que pasen desapercibidos por el cortafuegos.

El **escaneo Null** y **Xmas** también buscan evadir la protección de un cortafuegos básico. En estos escaneos, se envían paquetes sin establecer ningún flag TCP o con múltiples flags activados, respectivamente. Si el cortafuegos no espera la llegada de estos paquetes o no sabe cómo manejarlos, podrían dejarse pasar.

<pre><code># ESCANEO Fin
<strong>nmap -sF &#x3C;ip_address>
</strong>
# ESCANEO Null
nmap -sN &#x3C;ip_address>
 
# ESCANEO  Xmas
nmap -sX &#x3C;ip_address>
</code></pre>

### ESCANEO UDP

Cambiando de protocolo, podemos obtener diferentes resultados, debido principalmente a que este protocolo no siempre es bloqueado por los cortafuegos. Los últimos comandos no dieron los resultados esperados, vamos a probar ahora con este protocolo.

```
nmap -sU <ip_address>
```

### ESCANEO TCP ACK

Este tipo de escaneo es diferente al resto porque **no intenta determinar si los puertos están abiertos**. **El objetivo de este escaneo es detectar el tipo de cortafuegos** que tenemos delante.

La sonda enviada sólo contiene el flag ACK activo. Si el puerto no responde o devuelve un paquete ICMP “destination unreachable”, consideramos que el puerto esta filtrado por el cortafuegos. Si por el contrario, devuelve un paquete RST, se clasificaría como “unfiltered”, es decir, que es alcanzable.

```
nmap -sA -vv -n -p <puerto> <ip_address>
```

Cada opción significa

* \-sA -> realiza el escaneo TCP ACK
* \-vv -> modo verbose
* \-n -> evita el intento de resolución DNS inversa para acelerar el proceso
* \-p puerto -> indicamos el puerto sobre el cual queremos realizar el escaneo

## AYUDA DE CONSOLA

Pero os vamos a dejar alguna tabla con los más utilizados de todos ellos

| COMANDOS COMUNES        | UTILIDAD                                                                                                                                          |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| -iL file                | Se pasa un fichero con el listado de objetivos a escanear                                                                                         |
| -iR num                 | Escanea objetivos aleatorios                                                                                                                      |
| –exclude host           | Excluir equipos                                                                                                                                   |
| –excludefile file       | Se pasa un fichero con los objetivos a excluir                                                                                                    |
| -Pn                     | no realiza ping                                                                                                                                   |
| -sL                     | lista los equipos                                                                                                                                 |
| -sn                     | ping sweep                                                                                                                                        |
| -PR                     | ping arp                                                                                                                                          |
| -ps puerto              | ping arp al puerto o puertos especificados                                                                                                        |
| -PS puerto              | ping tcp ack al puerto o puertos especificados                                                                                                    |
| -PU puerto              | ping udp al puerto o puertos especificados                                                                                                        |
| -PY puerto              | ping sctp al puerto o puertos especificados                                                                                                       |
| -PE                     | ping icmp                                                                                                                                         |
| -PM                     | ping icmp address mask                                                                                                                            |
| -6                      | habilita el escaneo ipv6                                                                                                                          |
| -sS                     | escaneo TCP Syn                                                                                                                                   |
| -sT                     | escaneo TCP Connect                                                                                                                               |
| -sF                     | escaneo FIN                                                                                                                                       |
| -sN                     | escaneo Null                                                                                                                                      |
| -sX                     | escaneo Xmas                                                                                                                                      |
| -sU                     | escaneo UDP                                                                                                                                       |
| -sW                     | escaneo TCP Window                                                                                                                                |
| -sO                     | escaneo IP Protocol                                                                                                                               |
| -sA                     | realiza el escaneo TCP ACK                                                                                                                        |
| -vv                     | modo verbose                                                                                                                                      |
| -n                      | evita el intento de resolución DNS inversa para acelerar el proceso                                                                               |
| -p puerto               | indicamos el puerto sobre el cual queremos realizar el escaneo                                                                                    |
| **COMANDOS**            | **DNS**                                                                                                                                           |
| -PO protocolo           | escanea el protocolo especificado                                                                                                                 |
| -n/-R                   | Resolver DNS nunca/siempre                                                                                                                        |
| –dns-servers server     | Especifica los servidores a escanear                                                                                                              |
| –traceroute             | muestra además el trazado de ruta                                                                                                                 |
| -p min-max              | escanea el rango de puertos especificado                                                                                                          |
| –top-ports              | escanea los puertos más utilizados                                                                                                                |
| Escaneo                 |                                                                                                                                                   |
| -sV                     | identifica servicios y versiones                                                                                                                  |
| –allports               | escanea todos los puertos posibles                                                                                                                |
| –version-intensity num  | especifica la intensidad del escaneo de 0-9 (min-max), hay que tener cuidado ya que el uso de esta acción hace mucho ruido en la máquina objetivo |
| –version-light          | escaneo en intensidad 2                                                                                                                           |
| –version-all            | escaneo en intensidad 9                                                                                                                           |
| –version-trace          | traza la actividad del análisis de versiones                                                                                                      |
| **SISTEMA**             | **OPERATIVO**                                                                                                                                     |
| -A                      | activa la detección de OS, version, script y traceroute                                                                                           |
| -O                      | detecta el OS                                                                                                                                     |
| –osscan-limit           | limita la detección                                                                                                                               |
| –osscan-guess           | realiza un escaneo más agresivo                                                                                                                   |
| **EVASION DE FIREWALL** | **Y SUPLANTACION**                                                                                                                                |
| -f                      | fragmenta paquetes de 8 bits                                                                                                                      |
| -mtu                    | fragmenta paquetes múltiplos de 8 bits                                                                                                            |
| –daa-length             | gestiona el tamaño del paquete                                                                                                                    |
| –randomize-host         | utiliza objetivos aleatorios                                                                                                                      |
| -D host\[hostx]         | utiliza señuelos                                                                                                                                  |
| -S \<ip>                | envía paquetes a una ip destino específica                                                                                                        |
| –spoof-mac \<mac>       | envía tramas de ethernet a la mac objetivo                                                                                                        |
| -g puerto               | envía paquetes desde el puerto especificado                                                                                                       |
| -e interface            | define la interfaz de red a utilizar                                                                                                              |

