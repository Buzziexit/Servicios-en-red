El **DHCP Relay (Relevo DHCP)** es una función que facilita la distribución de direcciones IP y configuraciones de red en redes que están divididas en múltiples subredes o segmentos separados. En lugar de configurar un servidor DHCP en cada segmento de red, puedes utilizar un DHCP Relay para reenviar las solicitudes de configuración desde las subredes hacia un servidor DHCP centralizado.

El proceso funciona de la siguiente manera:

Un cliente en una subred emite una solicitud DHCP (Discover).

En lugar de ser atendida por un servidor DHCP local, la solicitud es interceptada por un agente DHCP Relay.

El agente DHCP Relay toma la solicitud y la reenvía a un servidor DHCP ubicado en una subred diferente, a través de mensajes llamados "Relay Forward" (Relevo hacia adelante).

El servidor DHCP recibe la solicitud y responde con la configuración adecuada.

La respuesta del servidor DHCP es reenviada de nuevo al cliente a través del agente DHCP Relay.

Esta operación permite que los clientes en diferentes subredes obtengan direcciones IP y configuraciones de red de un servidor DHCP centralizado, incluso si no están en la misma red física.

El uso del DHCP Relay es beneficioso en entornos donde la administración centralizada de direcciones IP es esencial, ya que simplifica la configuración y administración de la red al permitir que un servidor DHCP sirva a múltiples subredes.


## DHCP 

### Tarjetas:

- Adaptador puente.
- Red interna (red0)

### **1. Primero configuramos la red.**

sudo nano /etc/network/interfaces

Es importante usar 192.168.1.x/24 para la red interna y 172.26.x.x para el adaptador puente.

![image](https://github.com/Scosrom/Dhcp/assets/114906778/2ccfcafd-3088-4a6c-a085-6500f3a320b1)


### **2. Configurar dns:**

nano  /etc/resolv.conf

Se pueden usar los generales de Google

8.8.8.8
8.8.4.4


![image](https://github.com/Scosrom/Dhcp/assets/114906778/ca8b5727-1e73-4aa6-b04e-3bc932fb4a5a)



sudo systemctl restart networking
sudo systemctl status networking



### **3. Configuramos DHCP**

sudo apt update
sudo apt install isc-dhcp-server


Para configurar los servidores hay que entrar en el archivo:

sudo nano /etc/dhcp/dhcpd.conf

(En este caso solo se configura 1 servidor, no usamos failover, si queremos poner otro de respaldo entonces si haría falta).


![image](https://github.com/Scosrom/Dhcp/assets/114906778/a577c5e5-51ae-46b7-827e-ccddbdf4942f)


![image](https://github.com/Scosrom/Dhcp/assets/114906778/8dd948ce-72e4-4b0d-84c6-57f45874a175)




La ip de mi servidor es 192.168.1.1, por eso la ponemos en option routers.
He dejado la ip 192.168.1.2 libre por si necesitamos un servidor de respaldo (failover).

### **4. Asignamos tarjeta**

sudo nano /etc/default/isc-dhcp-server

Indicamos la tarjeta de red asignada al dhcp


![image](https://github.com/Scosrom/Dhcp/assets/114906778/9fc6a911-4939-491c-992e-a7dd58095382)



Configuramos para que el cliente tenga salida a internet (En este servidor dhcp)



### **5.Habilitar el Reenvío de Paquetes:**
Abre el archivo de configuración de sysctl:


sudo nano /etc/sysctl.conf

Asegúrate de que la siguiente línea esté descomentada o agrégala si no existe:


net.ipv4.ip_forward=1

Guarda y cierra el archivo. Luego, aplica los cambios con el siguiente comando:




sudo sysctl -p


### **6.Configurar NAT (Network Address Translation):**
Utiliza iptables para configurar NAT en la interfaz de red que tiene acceso a Internet (puente). Esto permitirá que el tráfico de la red interna se traduzca adecuadamente para salir a Internet.
Ejecuta los sigu
ientes comandos:


sudo iptables -t nat -A POSTROUTING -o <interfaz_puente> -j MASQUERADE

sudo iptables-save | sudo tee /etc/iptables/rules.v4

Asegúrate de reemplazar <interfaz_puente> con el nombre de tu interfaz de red puente (puede ser algo como enp0s3 o similar).

Para hacer que estos cambios sean persistentes después del reinicio, puedes instalar el paquete iptables-persistent:


sudo apt-get install iptables-persistent


sudo systemctl restart networking
sudo systemctl restart isc-dhcp-server


## dhcp relay

### Tarjetas
- Red interna (red0)
- Red interna (red0)


### **1. Configurar la red.** 

![image](https://github.com/Scosrom/Dhcp/assets/114906778/4e10e5b7-97d8-4c9c-bf4c-0bcdbfe5b6ed)




### **2. Instalamos servidor dhcp-relay**

apt install isc-dhcp-relay

En la primera opción que nos aparece tenemos que marcar la IP del servidor DHCP, en este caso sería 192.168.1.1

En la segunda opción marcamos la tarjeta de red por la que va a escuchar las peticiones dhcp, es decir, por la tarjeta enp0s3 recibe señal del servidor dhcp y por la tarjeta enp0s8 realiza las escuchas y traspasa estas peticiones al servidor dhcp.  

En la tercera opción en principio no marcamos nada. 

Despues de marcar esto no hace falta entrar en el fichero, pero si queremos comprobar que este todo correcto entrar en:


sudo nano /etc/default/isc-dhcp-relay

![image](https://github.com/Scosrom/Dhcp/assets/114906778/a444cfd4-e1c2-48d8-a621-1100b54f00d1)




sudo systemctl restart isc-dhcp-relay

sudo systemctl status isc-dhcp-relay

### **3. Configurar para que el cliente salga a internet.**

En este servidor no hay que usar los comandos iptables, solo haría falta lo siguiente:

sudo nano /etc/sysctl.conf

Asegúrate de que la siguiente línea esté descomentada o agrégala si no existe:

net.ipv4.ip_forward=1

sudo sysctl -p

sudo systemctl restart isc-dhcp-relay
sudo systemctl restart networking.service

## Cliente 

### Tarjetas
- red interna (red0)

### **1. Configuramos la red**

nano /etc/network/interfaces

![image](https://github.com/Scosrom/Dhcp/assets/114906778/9d6f446a-fe42-4846-953a-366912503a37)
