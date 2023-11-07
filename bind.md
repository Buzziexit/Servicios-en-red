# COnfiguracion DNS con Bind9

## **Paso 1: Instalación de BIND9**

Primero, procedí a instalar el servidor DNS BIND9. Para ello, actualicé los paquetes y realicé la instalación utilizando el gestor de paquetes apt.

```
sudo apt update
sudo apt install bind9
```

## **Paso 2: Configuración del archivo named.conf.options**

### **1. Versión del Servidor (Version):**


`version "No disponible";`

Esta directiva oculta la versión específica de BIND9, proporcionando una capa adicional de seguridad.


### **2. Configuración de la Escucha en el Puerto 53:**


`listen-on port 53 { any; };`

Esta configuración indica que BIND9 debe escuchar en el puerto estándar 53, utilizado para las consultas DNS. { any; } especifica que debe escuchar en todas las interfaces disponibles.

### **3. Recursividad (Recursion):**


`recursion yes;`

`allow-recursion { localnets; };`

Habilité la recursividad, lo que permite que mi servidor DNS resuelva consultas en nombre de los clientes. Además, limité las consultas recursivas a las redes locales.

### **4. Transferencia de Zona (Zone Transfer):**

`allow-transfer { none; };`

Deshabilité la transferencia de zona hacia otros servidores DNS, lo que significa que mi servidor no permitirá que otros servidores soliciten copias de mis zonas.

![1](/img/1.PNG)

## **Paso 3: Edición de named.conf.local**

1. En el archivo named.conf.local, he agregado configuraciones específicas para mis zonas de dominio.

2. Primero, he abierto el archivo named.conf.local con un editor de texto.

3. Luego, he comenzado definiendo una zona para mi dominio sandracostales.com.

4. He utilizado la siguiente sintaxis:

```
zone "sandracostales.com" in {
    type master;
    file "db.master.sandracostales.com.";
    #allow-transfer { 192.168.1.3; };
};
```

5. Explicando la configuración, he indicado que esta zona es del tipo maestra (master), lo que significa que mi servidor será la fuente principal de información para esta zona.

6. También he especificado el archivo de configuración de la zona como db.master.sandracostales.com..

7. Además, he comentado la línea #allow-transfer { 192.168.1.3; }; para evitar transferencias de zona hacia otro servidor en este momento.

8. Después, he definido una zona inversa para las direcciones IP en mi red local 192.168.1.x.

9. He utilizado la siguiente sintaxis:

```
zone "1.168.192.IN-ADDR.ARPA" in {
    type master;
    file "db.master.192.168.1.rev";
    #allow-transfer { 192.168.1.3; };
};
```

10. He indicado que esta zona es del tipo maestra (master) y he especificado el archivo de configuración de la zona como db.master.192.168.1.rev.

11. También he comentado la línea #allow-transfer { 192.168.1.3; }; para evitar transferencias de zona hacia otro servidor en este momento.

![2](/img/2.PNG)





## **Paso 4:  Configuración de Zona Directa**

En el archivo db.master.sandracostales.com, he establecido las entradas correspondientes para mi dominio sandracostales.com, incluyendo registros de tipo NS, A y CNAME

**1. Inicio del Archivo y Definición de TTL:**

```
$TTL 2d
$ORIGIN sandracostales.com.
```

- He establecido el tiempo de vida del registro (TTL) en 2 días (2d).`

- También he definido el dominio base como sandracostales.com..

**2. Definición del Registro SOA (Start of Authority):**


```
@         IN    SOA   ns1.sandracostales.com. hostmaster.sandracostales.com. (
                   2016040800  ; se = número de serie
                   12h         ; ref = tiempo de refresco
                   15m         ; ret = tiempo de reintento de refresco
                   3w          ; ex = tiempo de expiración
                   2h          ; nx = tiempo de caché para registros no encontrados
                   )
```
- He especificado que ns1.sandracostales.com. es el servidor de nombres principal (master).

- También he indicado hostmaster.sandracostales.com. como la dirección de correo electrónico del responsable de la zona.

- Los números representan diferentes parámetros de la zona, como el número de serie, tiempos de refresco, tiempos de reintento, tiempo de expiración y tiempo de caché.


**3. Definición de Registros NS:**

```
IN    NS    ns1.sandracostales.com.
IN    NS    ns2.sandracostales.com.
```
- He establecido los servidores de nombres autorizados para esta zona.

**4. Definición de Registros A:**

```
ns1       IN    A         192.168.1.2
ns2       IN    A         192.168.1.3
prof      IN    A         192.168.1.2
www       IN    A         192.168.1.2
```

- He asignado nombres de host (por ejemplo, ns1) a direcciones IP (por ejemplo, 192.168.1.2).

**5. Definición de un Registro CNAME:**

```
ftp       IN    CNAME     www
```

- He establecido que el nombre ftp es un alias (CNAME) para www.sandracostales.com.

![3](/img/3.PNG)

## **Paso 5: Configuración de Zona Inversa**

En el archivo db.master.192.168.1.rev, he definido las entradas PTR que asocian direcciones IP con nombres de host, lo que facilita la resolución inversa.

**1. Inicio del Archivo y Definición de TTL:**

```
$TTL 2d
$ORIGIN 1.168.192.IN-ADDR.ARPA.
```

- He establecido el tiempo de vida del registro (TTL) en 2 días (2d).

- También he definido el dominio base como 1.168.192.IN-ADDR.ARPA..

**2. Definición del Registro SOA (Start of Authority):**

```
@         IN    SOA   ns1.sandracostales.com. hostmaster.sandracostales.com. (
                   2016040800  ; se = número de serie
                   12h         ; ref = tiempo de refresco
                   15m         ; ret = tiempo de reintento de refresco
                   3w          ; ex = tiempo de expiración
                   2h          ; nx = tiempo de caché para registros no encontrados
                   )
```
- He especificado que ns1.sandracostales.com. es el servidor de nombres principal (master).

- También he indicado hostmaster.sandracostales.com. como la dirección de correo electrónico del responsable de la zona.

- Los números representan diferentes parámetros de la zona, como el número de serie, tiempos de refresco, tiempos de reintento, tiempo de expiración y tiempo de caché.

**3. Definición de Registros PTR:**

```
2       IN      PTR     ns1.sandracostales.com.
3       IN      PTR     ns2.sandracostales.com.
```

- He asociado las direcciones IP (192.168.1.2 y 192.168.1.3) con los nombres de host correspondientes.

![4](/img/4.PNG)

Con estos pasos, he logrado configurar BIND9 en mi sistema Debian 12 para funcionar como un servidor DNS capaz de resolver nombres de dominio tanto en sentido directo como inverso.



# Configurar servidor esclavo

## **Paso 1: Configuración del archivo named.conf.local en el servidor esclavo**


```
zone "sandracostales.com" in {
   type slave;
   file "db.slave.sandracostales.com";
   masters { 192.168.1.2; }; // Aquí especifico la IP del servidor maestro
};

zone "1.168.192.IN-ADDR.ARPA" in {
   type slave;
   file "db.slave.192.168.1.rev";
   masters { 192.168.1.2; }; // Aquí especifico la IP del servidor maestro
};
```



![6](/img/6.PNG)

-  En named.conf.local, se definen las zonas directa e inversa para el dominio "sandracostales.com". Ambas zonas están configuradas como "master", lo que indica que este servidor es el maestro para esas zonas.

## Paso 2: Configuración del archivo named.conf.options en el servidor esclavo

```
options {
    directory "/var/cache/bind";

    version "No disponible";

    listen-on port 53 { any; };
    recursion yes;
    allow-recursion { localnets; };

    allow-transfer { none; };
    notify no;
};
```


![5](/img/5.PNG)

- En el archivo named.conf.options, he especificado las opciones generales para el servidor DNS. Se permite la recursión (búsqueda de direcciones IP a partir de nombres de dominio) y se limita a las redes locales. No se permiten transferencias de zona.

## **Paso 3: Configuración del archivo db.slave.sandracostales.com en el servidor esclavo**

```
$TTL 2d
$ORIGIN sandracostales.com.

@       IN    SOA   ns1.sandracostales.com. hostmaster.sandracostales.com. (
                    2016040800  ; Serial number
                    12h         ; Refresh
                    15m         ; Retry
                    3w          ; Expire
                    2h          ; Negative caching TTL
                  )
        IN    NS    ns1.sandracostales.com.
        IN    NS    ns2.sandracostales.com.
ns1     IN    A     192.168.1.2
ns2     IN    A     192.168.1.3
prof    IN    A     192.168.1.2
www     IN    A     192.168.1.2
ftp     IN    CNAME www
```
![7](/img/7.PNG)

- En el archivo db.master.sandracostales.com, se especifican los registros para el dominio "sandracostales.com", incluyendo la configuración del servidor de nombres (NS), direcciones IP (A) y alias de dominio (CNAME).

## **Paso 4: Configuración del archivo db.slave.192.168.1.rev en el servidor esclavo**

```
$TTL 2d
$ORIGIN 1.168.192.IN-ADDR.ARPA.

@       IN    SOA   ns1.sandracostales.com. hostmaster.sandracostales.com. (
                    2016040800  ; Serial number
                    12h         ; Refresh
                    15m         ; Retry
                    3w          ; Expire
                    2h          ; Negative caching TTL
                  )
        IN    NS    ns1.sandracostales.com.
        IN    NS    ns2.sandracostales.com.
2       IN    PTR   ns1.sandracostales.com.
3       IN    PTR   ns2.sandracostales.com.
```

![8](/img/8.PNG)

En el archivo db.master.192.168.1.rev, se establecen las entradas PTR para asociar direcciones IP con nombres de host, permitiendo la resolución inversa.

## **Comprobación:**

![9](/img/9.PNG)
