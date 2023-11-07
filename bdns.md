# Configuración de un Servidor DNS Integrado con un Servidor DHCP

1. Primero, asegurémonos de tener dos máquinas Linux en modo RED INTERNA para que tengan visibilidad entre ellas. Es importante que el servidor DHCP no afecte a la red del sistema anfitrión.

2. Desinstalemos el paquete apparmor si está instalado:

```
sudo apt-get purge apparmor
```

3. Vamos a configurar BIND9 e ISC-DHCP-SERVER de forma independiente. Verifiquemos que ambos funcionen correctamente antes de continuar.

4. Establezcamos la dirección estática del servidor. En mi caso, he configurado el servidor con la dirección IP 192.168.1.1 y he establecido el servidor DNS en 127.0.0.1.

5. Asegurémonos de que el nombre de la máquina en Linux esté correctamente configurado en /etc/hostname.

6. Modifiquemos el archivo /etc/bind/rndc.key para verificar la clave que el servidor DHCP utilizará para comunicarse con el servidor BIND.

7. En el archivo named.conf.local, añadiremos las siguientes líneas para permitir peticiones desde localhost usando la clave rndc-key:

```
include "/etc/bind/rndc.key";

controls {
   inet 127.0.0.1 port 953
   allow { 127.0.0.1; } keys { "rndc-key"; };
};

zone "ieslapaloma.com" {
   type master;
   file "/etc/bind/ieslapaloma.db";
   allow-update { key "rndc-key"; };
   notify yes;
};

zone "1.168.192.in-addr.arpa" {
   type master;
   file "/etc/bind/192.db";
   allow-update { key "rndc-key"; };
   notify yes;
};

```

8. Ahora, modificamos el archivo /etc/dhcp/dhcp.conf con los parámetros adecuados para que el servidor DHCP actualice el servidor DNS:

```
server-identifier ns.ieslapaloma.com;
ddns-updates on;
ddns-update-style interim;
ddns-domainname "sandracostales.com.";
ddns-rev-domainname "in-addr.arpa.";
deny client-updates;
include "/etc/bind/rndc.key";

zone ieslapaloma.com. {
  primary 127.0.0.1;
  key rndc-key;
}

zone 1.168.192.in-addr.arpa. {
   primary 127.0.0.1;
   key rndc-key;
}


```

segúrate de que en server-identifier pongas el nombre del servidor DNS y que sea un nombre completo que se pueda resolver.

También comenta la línea en el archivo que dice ddns-update-style none;.

9. Configuramos una subred con algo similar a:

```
subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.20 192.168.1.250;
  option routers 192.168.1.10;
  option domain-name "sandracostales.com.";
}

```

10. Detenemos los servicios BIND9 y ISC-DHCP-Server:

```
sudo service bind9 stop
sudo service isc-dhcp-server stop
```

11. Borramos los archivos de concesiones de DHCP para asegurarnos de que se asignen nuevas direcciones a los clientes:

```
sudo echo "" > /var/lib/dhcp/dhcpd.leases
sudo echo "" > /var/lib/dhcp/dhcpd.leases~
```

12. Permitimos que el servidor DHCP pueda leer el archivo rndc.key:

sudo chown bind:dhcpd /etc/bind/rndc.key

13. Cambiamos los permisos para que BIND pueda escribir en /etc/bind:

sudo chown -R bind /etc/bind

14. Finalmente, iniciamos nuevamente los servicios:

```
sudo service bind9 start
sudo service isc-dhcp-server start
```

15. Reiniciamos los clientes y verificamos el archivo de registro
