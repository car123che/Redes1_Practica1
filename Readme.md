### Universidad de San Carlos
### Facultad de Ingeniería
### Redes de computadoras 1
### Sección O
### Carlos Agustín Ché Mijangos
### 201800624
### Segundo semestre 2021

# Manual De Configuración 
El presente proyecto trata sobre la configuración y conexión a una VPN y que los equipos conectados a la VPN puedan comunicarse entre si. 

Se utilizó el servicio de nube de Google Cloud para crear una máquina virtual la cual actúa como conector de VPN para acceder a la red.

Y se instaló un cliente VPN para poder conectarse a la red.

La red creada es la siguiente:
![red](img/DiagramaRed.png)

## Qué es una VPN
Una red privada virtual ( VPN ) extiende una red privada a través de una red pública y permite a los usuarios enviar y recibir datos a través de redes públicas o compartidas como si sus dispositivos informáticos estuvieran conectados directamente a la red privada. Las aplicaciones que se ejecutan a través de una VPN pueden, por tanto, beneficiarse de la funcionalidad, la seguridad y la gestión de la red privada. 

## Qué es ICMP
El Protocolo de mensajes de control de Internet ( ICMP ) es un protocolo de apoyo en el conjunto de protocolos de Internet . Lo utilizan los dispositivos de red , incluidos los enrutadores , para enviar mensajes de error e información operativa que indica el éxito o el fracaso al comunicarse con otra dirección IP , por ejemplo, se indica un error cuando un servicio solicitado no está disponible o que un host o enrutador no pudo ser alcanzado. ICMP se diferencia de los protocolos de transporte como TCP y UDP.en el sentido de que no se utiliza normalmente para intercambiar datos entre sistemas, ni las aplicaciones de red del usuario final lo emplean habitualmente.

Para esta red se utilizo el protoclo UDP
## Herramientas Utilizadas
* Servicio de nube Google Cloud
* Una instancia de VM en la nube google cloud (Sistema Operativo usado Ubuntu)
* Administrador de VPNs OpenVPN
* 2 computadoras físicas
* 2 máquinas virtuales

En este caso las 4 máquinas tienen sistema operativo Windows, pero se puede utilizar cualquier sistema operativo.

## Creación de la instancia en Google Cloud
Al iniciar sesión en Google Cloud, la plataforma incial es la siguiente:
![Cloud Inicio](img/Inicio.png)

En la barra superior podemos ubicar el nombre del proyecto en cual estamos trabajando
![Proyecto](img/Proyectos.png)

Al dar click se desplegara la siguiente ventana:
![Proyecto Nuevo](img/nuevoP.png)
para crear un proyecto nuevo, debemos dar click en el boton de proyecto nuevo. 

Se desplegará la siguiente ventana:
![Nuevo Proyecto](img/proyectoN.png)
Debemos llenar los campos solicitados y dar click en CREAR.

Al hacer esto se creará un proyecto nuevo y podremos ver el nombre del proyecto en el centro de la barra superior azul en la página de inicio.

Luego debemos desplegar el menú que se encuentra en la barra superior azul, en la esquina izquierda.

Se desplegará el siguiente menú, donde primero debemos seleccionar la opción COMPUTE ENGINE y luego la opción INSTANCIAS DE VM.
![menu](img/menu.png)

Luego se mostrara la siguiente página en donde debemos seleccionar la opción crear una nueva instancia:
![nueva instancia](img/crearI.png)

Se desplegará la siguiente páginas, en donde debemos llenar los campos solicitados, en este caso el nombre ingresado fue conector.
![nombre instancia](img/instanciaN.png)

En el tipo de máquina escogemos la opción e2-micro ya que es la opción más barata.
![tipo máquina](img/tipoM.png)

También podemos escoger el sistema operativo deseado, en esta ocasión se escogio el sistema operativo Debian GNU/Linux 10.
Además debemos de seleccionar las opciones de permitir el tráfico HTTP y HTTPS. 
![configuracion](img/config.png)
Por último damos click en crear para crear la instancia con las configuracioens dadas, las demás opciones no se modificaron.

A continuación tenemos nuestra instancia creada, y podemos vizualizar y sus detalles en la página de instancias de vm.
![instancia](img/instancia.png)

Para poder acceder a la instancia utilizamos la opcion de ssh de google la cual abre la consola de la máquina virtual en el navegador. Solo debemos dar click en la siguiente opción
![ssh](img/ssh.png)

Y la consola de la máquina virtual que se abre en el navegador es la siguiente;
![Máquina Virtual](img/maquinaV.png)

## Permisos de la máquina virtual
Debemos habilitar los siguientes permisos para permitir el la entrada y salida de datos.
Para eso vamos al menu de Cloud y seleccionamos la siguiente opcion:

![Firewall](img/fire.png)

Al desplegar la nueva ventana, seleccionamos la opcion de Crear Regla de Firewall

### Permisos de entrada

![Entrada](img/allin.png)

Para crear las reglas de permisos de entrada primero ingresamos el nombre de la regla, luego debemos seleccionar que la dirección del tráfico sea de entrada. En destinos escogemos la opción de todas las instancias de red. Luego en los rangos de IP ingresamos la IP 0.0.0.0/0 para que permita todos los rangos.
Y en protocolos escogemos la opción de UDP y le asignamos el puerto 1194.

Por ultimo damos click en crear.

### Permisos de salida

Para crear las reglas de los permisos de salida, la configuración es la misma que los de entrada solo en dirección de tráfico escogemos la opción de salida.

## Configuración de la instancia virtual de cloud

Al tener nuestra máquina virtual creada en la nube deemos ingresar los siguientes comando en el orden en el que aparecen:

* `sudo apt-get update`
* `sudo apt-get install wget`
* `sudo wget https://cubaelectronica.com/OpenVPN/openvpn-install.sh`

Con esto tenemos el servidor de VPN instaldo el máquina virtual y ahora vamos a proceder a configurarlo.

Primero debemos ingresar el comando:
`sudo bash openvpn-install.sh`

Luego se desplegara la siguiente información en consola, y debemos llenar los campos solicitados:

![ips](img/ips.png)
Podemos ver que primero nos muestra una IP, esta IP es la IP interna de nuestra máquina virtual, luego nos solicita una IP pública o hostname y debemos ingresar la IP externa de nuestra máquina virtual.

Luego se configuraran otros datos adicionales, los cuales se dejaron los default para un mejor rendimiento.
![configuracion](img/configMV.png)

Por último se procedio a crear el primer usuario para la VPN.
![Usuario 1](img/user1.png)

Para crear usuarios nuevos lo cuales se puedan conectar a la VPN, debemos de ingresar el comando 
`sudo bash openvpn-install.sh`
luego nos desplagara el menú debemos seleccionar la opción agregar nuevo usuario y llenar los campos solicitados de la siguiente manera:
![Nuevo Usuario](img/newUser.png)

De esta manera creamos los 4 usuarios con los siguientes nombres:

* carlos
* clientedos
* clientetres
* clientecuatro

Y para poder acceder a la VPN desde nuestros equipos debemos descargar los archivos .ovpn los cuales son las claves de los usuarios y con estas podemos acceder desde la interfaz del programa OpenVPN client.

Para descargar los archivos debemos ingresar los siguientes comandos en consola:

* `cd ..`
* `sudo -i`
* `cp clientedos.ovpn /home/`
* `cp clientetres.ovpn /home/`
* `cp clientecuatro.ovpn /home/`

Con esto hemos copiado los archivos de clientedos, clientetres, clientecuatro al hombe para asi poder descargarlos posteriormente, el archivo carlos.ovpn al ser el primero creado ya se encontraba en el home. 

Para poder descargar los archivos accedemos al menu que se despliega al seleccionar la tuerca en la esquina superior derecha y le damos click en la ocpión descargar

![descargar](img/save.png)


Por útlimo se mostrar la siguiente ventana en donde debemos ingresar la ruta del archivo a descargar en este caso la ruta y el archivo son /hombe/carlos.ovpn

![descarga](img/Descarga.png)

Realizar el paso anterior con los otros tres archivos de las claves de los clientes.

## Configuración de la herramienta de VPN
Para poder realizar la conexión a la VPN abrimos la aplicación de OpenVPN Connect. Vamos a la opcion de FILE y damos click en Browse para buscar el archivo con la clave para el usuario.
![OpenVPN](img/open.png)

Al tener la clave cargada aparecera la siguiente ventana y damos click en conectar
![Conectar](img/connect.png)

Al estar conectados se mostrará la siguiente interfaz
![Conexión](img/c.png)

Para verificar la conexión a la VPN vamos ala página de cual es mi IP y podemos observar que la IP que muestra es la misma IP pública de nuestra máquina virtual
![ip](img/ip.png)

La conexión para los demás máquina y en los otros usuarios es la misma, solo se debe escoger la clave del cliente que se desea al momento de buscar el archivo .ovpn

## Red Privada
Luego de conectar las 4 máquina a la VPN, estas pueden comunciarse entre si, teniendo además un IP nueva la cuál corresponde a la red privada a la que estan conectadas, a continuación se muestra la IP de las máquina conectadas.

(Para saber la ip de nuestras máquinas ya que estas son del sistema operativo windows ingresamos el comando `ipconfig` ).

### IPS de las 4 máquina conectadas

|   Cliente Carlos  | Cliente Dos   |
|   -------------   |   ---------   |
| ![Carlos](img/IP1.png) | ![Carlos](img/IP2.png)  |

|   Cliente Tres  | Cliente Cuatro   |
|   -------------   |   ---------   |
| ![Carlos](img/IP3.png) | ![Carlos](img/IP4.png)  |

### Comunicación con el ambiente virtual en la nube

A continuación se muestra la comunicación entre la máquina virtual en la nube con las máquina conectadas a la vpn. 
Para esto debemos simplemente correr el comando `ping IP` donde la IP es la IP de cada máquina conectada a la VPN, las cuales se encuentra detalladas anteriomente.

|   Comunicación con Carlos  | Comunicación con ClienteDos   |
|   -------------   |   ---------   |
| ![Carlos](img/SSH-1.png) | ![Carlos](img/SSH-2.png)  |

|   Comunicación con ClienteTres  | Comunicación con ClienteCuatro   |
|   -------------   |   ---------   |
| ![Carlos](img/SSH-3.png) | ![Carlos](img/SSH-4.png)  |

En la siguientes imagenes podemos observar la comunicación en sentido contrario dando ping desde cada máquina conectada a la VPN con la máquina virtual. De igual manera solo debemos ingresar el comando `ping IP`.

Para conocer la IP de nuestra instancia en la nube ya que esta esta en el sistema operativo linux, ingresamos el comando 
`ip address`. Lo cual nos duelve el siguiente resultado y la IP de nuestra máquina virtual.

![IP maquina virtual](img/IPMV.png)

|   Carlos - MV  |  ClienteDos -MV   |
|   -------------   |   ---------   |
| ![Carlos](img/1-SSH.png) | ![Carlos](img/2-SSH.png)  |

|  ClienteTres - MV  | ClienteCuatro - MV  |
|   -------------   |   ---------   |
| ![Carlos](img/3-SSH.png) | ![Carlos](img/4-SSH.png)  |

### Comunicación entre máquinas 
A continuación se muestra la comunicación entre las máquina conectadas a la VPN.

|  Carlos - Clientedos  | Carlos - Clientetres  |
|   -------------   |   ---------   |
| ![Carlos](img/1-2.png) | ![Carlos](img/1-3.png)  |

|  Carlos - Clientecuatro  | Clientedos - Cliente carlos  |
|   -------------   |   ---------   |
| ![Carlos](img/1-4.png) | ![Carlos](img/2-1.png)  |

|  Clientedos - Cliente tres  | Clientedos - Clientecuatro  |
|   -------------   |   ---------   |
| ![Carlos](img/2-3.png) | ![Carlos](img/2-4.png)  |


|  Cliente tres - Cliente carlos  | Cliente tres - Cliente dos  |
|   -------------   |   ---------   |
| ![Carlos](img/3-1.png) | ![Carlos](img/3-2.png)  |


|  Cliente tres - Cliente cuatro  | Cliente cuatro - Cliente Carlos  |
|   -------------   |   ---------   |
| ![Carlos](img/3-4.png) | ![Carlos](img/4-1.png)  |

|  Cliente cuatro - Cliente dos  | Cliente cuatro - Cliente tres  |
|   -------------   |   ---------   |
| ![Carlos](img/4-2.png) | ![Carlos](img/4-3.png)  |



## Anexos 
* [Creación de una cuenta en Google Cloud](https://www.youtube.com/watch?v=UEW-S7pWCiw)
* [Link de descarga de OpenVpn](https://openvpn.net/vpn-client/)
* [Guía para la instalación de OpenVpn](https://www.youtube.com/watch?v=mWZdmPhQeyc)