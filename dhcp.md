El **DHCP Failover** (o conmutación por fallo de DHCP) es una característica que proporciona redundancia y alta disponibilidad en un entorno DHCP. Esto significa que si un servidor DHCP falla, otro servidor puede asumir sus funciones sin interrupciones en el servicio.

Existen diferentes formas de implementar la conmutación por fallo en DHCP:

**1. Hot Standby:** En este modo, un servidor DHCP está activo y el otro está en espera (standby). El servidor activo responde a las solicitudes de los clientes mientras que el servidor en espera se mantiene en un estado de inactividad, listo para tomar el control si el servidor activo falla.

**2. Load Balancing:** En este modo, ambos servidores DHCP están activos y distribuyen las solicitudes de los clientes entre ellos. Esto ayuda a distribuir la carga de trabajo y mejora el rendimiento del servicio DHCP.

**3. Round Robin:** Similar al load balancing, pero en lugar de basarse en la carga, los servidores DHCP responden en un ciclo secuencial.

**4. Manual Failover:** En este caso, la conmutación por fallo se lleva a cabo manualmente por un administrador cuando uno de los servidores DHCP falla.

La elección del método de conmutación por fallo depende de los requisitos de disponibilidad y carga de trabajo de la red. Cada método tiene sus propias ventajas y consideraciones. Por ejemplo, el hot standby proporciona una conmutación rápida y automática en caso de fallo, pero requiere un servidor en espera que no esté en uso. Load balancing distribuye la carga, pero requiere una configuración más compleja.

En resumen, DHCP y DHCP Failover son herramientas esenciales en la gestión de direcciones IP en una red, y el failover es una estrategia importante para garantizar la disponibilidad del servicio en caso de fallos.
## Configurar red:

**Archivo de Interfaces:**

<code>sudo nano /etc/network/interfaces </code>

Asegúrate de utilizar la dirección IP 192.168.1.x/24 para la red interna y 172.26.x.x para el adaptador puente.

Enp0s3 es mi red interna

Enp0s8 es mi adaptador puente

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/5b585643-8efd-48e5-b3f9-1164a5c1a2b0)


***Configurar dns:***

**Archivo de DNS:**

<code> nano  /etc/resolv.conf </code>

Puedes usar los servidores DNS de Google:

8.8.8.8
8.8.4.4


![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/bb3de59e-8d40-4b19-a074-5abab8d9f33d)


<code>sudo systemctl restart networking </code>


<code>sudo systemctl status networking </code>



## Servidor hdcp

<code> sudo apt update </code>

<code> sudo apt install isc-dhcp-server </code>


Edita el archivo de configuración del servidor DHCP:


<code> sudo nano /etc/dhcp/dhcpd.conf </code>

Realiza la siguiente modificación en cada servidor para asignar el principal y secundario:

Address (IP del servidor)
Peer address (IP del otro servidor)

### Configuración en el Servidor Primario:

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/19329cc2-2626-438c-96d4-5d39b3a2cc7e)

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/65f97689-7fae-450f-9df8-3b025017b587)

### Configuración en el Servidor Secundario:

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/7eeb6899-2fcd-4eeb-94ec-798a9663b966)

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/24715dfb-bc73-4238-9fad-93dc57786f4f)


Edita el archivo de configuración por defecto del servidor DHCP:


<code> sudo nano /etc/default/isc-dhcp-server </code>

Indica la tarjeta de red asignada al DHCP.


![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/e2725e79-3701-47da-9ef5-c914e67c6a97)

Verifica con systemctl.

## Configurar el Cliente:

1. En VirtualBox, asegúrate de configurar la red interna correctamente para que la máquina cliente pueda conectarse. Abre la máquina virtual una vez que la configuración esté lista.

2. Asegúrate de que el cliente obtenga su configuración de red a través del servidor DHCP. Además, verifica que la puerta de enlace esté configurada correctamente, utilizando la interfaz interna del servidor Debian.

**Reinicia los Servicios:**


<code> sudo systemctl restart networking </code>

<code> sudo systemctl restart isc-dhcp-server </code>

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/8f0bbb0c-9a38-4a76-ae13-ea5bdfeb598f)

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/66e30fbc-30cb-4c15-8283-e043ebfc52dd)


## Habilitar el Reenvío de Paquetes (Servidor) :

Abre el archivo de configuración de sysctl:


<code> sudo nano /etc/sysctl.conf </code>

Asegúrate de que la siguiente línea esté descomentada o agrégala si no existe:


<code> net.ipv4.ip_forward=1 </code>

Guarda y cierra el archivo. Luego, aplica los cambios con el siguiente comando:


<code> sudo sysctl -p </code>

**Configurar NAT (Network Address Translation):**

Utiliza iptables para configurar NAT en la interfaz de red que tiene acceso a Internet (puente). Esto permitirá que el tráfico de la red interna se traduzca adecuadamente para salir a Internet.


<code> sudo iptables -t nat -A POSTROUTING -o <interfaz_puente> -j MASQUERADE </code>

<code> sudo iptables-save | sudo tee /etc/iptables/rules.v4 </code>

Asegúrate de reemplazar <interfaz_puente> con el nombre de tu interfaz de red puente (puede ser algo como enp0s3 o similar).

Para hacer que estos cambios sean persistentes después del reinicio, puedes instalar el paquete iptables-persistent:


<code>  sudo apt-get install iptables-persistent </code>






### Error de sintaxis:

Si se presenta un error de sintaxis, puedes verificar la configuración con el siguiente comando.

<code> dhcpd -t -cf /etc/dhcp/dhcpd.conf </code>
