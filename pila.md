# Despliegue en AWS

***En esta practica vamos a utilizar:***

- SO Debian 12
- Servidor apache2
- Php
- Base de datos MariaDB

## 1. Creamos servidor en AWS

1. Elegimos SO y tipo de instancia
   
El sistema operativo que vamos a Utilizar es Debian 12
Tipo de instancia : t2.micro

![1](https://github.com/Scosrom/practicas/assets/114906778/26cfe80c-29ec-4d45-b194-8eef99e41d45)

![2](https://github.com/Scosrom/practicas/assets/114906778/d93ffb6b-c003-4511-aaae-1d63332ba369)

2. Creamos un par de claves:

![3](https://github.com/Scosrom/practicas/assets/114906778/7e999806-520e-4221-a318-ccfbcdb28ccf)

3. Configuramos la red:

![4](https://github.com/Scosrom/practicas/assets/114906778/5a039b98-6459-48a2-a6c7-199f469d2b23)

En esta practica no vamos a modificar los valores de la red por lo tanto podemos asignar la red (default).

4. Configuramos los puertos:

Como vamos a utilizar el servicio http tenemos que abrir los puertos, en este caso el puerto 80.
En origen de la conexión vamos a indicar que dispositivos se pueden conectar a nuestro puerto, 0.0.0.0/0 reconoce que cualquier IP se puede conectar a nuestro servicio. Sería más seguro indicar la IP desde la que me voy a conectar. 

![5](https://github.com/Scosrom/practicas/assets/114906778/102a3e65-f0c8-4240-9c98-875ae3939255)

5. Lanzamos instancia y nos conectamos por SSH

![6](https://github.com/Scosrom/practicas/assets/114906778/71043521-ee33-440b-8872-9b6d57839d1e)

Desde Terminal

(Es importante ejecutar el comando desde la carpeta donde tenemos miclave.pem)

![7](https://github.com/Scosrom/practicas/assets/114906778/d3df5d5e-6f99-45f4-ab7a-d99850f9d620)

## Instalamos apache2

<code> apt update </code>

<code> apt upgrade -y </code>

<code> apt install apache2 -y </code>

Comprobamos que el servicio este activo

<code> systemctl status apache2 </code>

![8](https://github.com/Scosrom/practicas/assets/114906778/57022d50-19a1-4618-9ad0-227346a1a115)


Verificamos que nuestro servicio este activo. 

## 2. Instalamos MariaDB

<code> apt install mariadb-server </code>

Accedemos a MariaDB, como aún no hemos configurado ninguna contraseña podemos entrar con admin y sin contraseña.

<code> mariadb -u root -p </code>

1. Creamos una Base de datos

<code> CREATE DATABASE lamp_db CHARSET utf8mb4; </code>

2. Entramos en la BD

   <code> USE lamp_db; </code>

3. Creamos Tabla
```
  CREATE TABLE users (
  id int(11) NOT NULL auto_increment,
  name varchar(100) NOT NULL,
  age int(3) NOT NULL,
  email varchar(100) NOT NULL,
  PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

   

4. Creamos un Usuario
   
<code> CREATE USER IF NOT EXISTS 'lamp_user'@'%'; </code>


5. Cambiamos la contraseña

<code> ALTER USER 'lamp_user'@'%' IDENTIFIED BY 'lamp_password'; </code>

6. Concedemos Privilegios.
  ``` 
   GRANT ALL PRIVILEGES ON lamp_db.* TO 'lamp_user'@'%';
*/
```

7. Actualizamos privilegios.

<code> FLUSH PRIVILEGES; </code>


## 3. Instalamos php y phpmyadmin

<code> apt install php -y </code>

<code> apt install phpmyadmin -y </code>

1. Marcamos el servicio apache2

![9](https://github.com/Scosrom/practicas/assets/114906778/e68032f4-cd17-4df5-bd95-b394f956bddc)

2. Añadimos una contraseña y la confirmamos:

![10](https://github.com/Scosrom/practicas/assets/114906778/e9ecb251-0cec-4626-8d1b-010bf13a3a8f)


![11](https://github.com/Scosrom/practicas/assets/114906778/cab47573-c09f-45d5-afc1-58018cc54a38)

## 5. Congifuramos el archivo /etc/www/html

1. Descargamos archivo scr del repositorio de github. 

<code> git clone https://github.com/Scosrom/php-MariaDB-Apache2.git </code>

2. Vamos hasta el la carpeta src y ejecutamos el siguiente comando para pegar todo el contenido en /var/www/html

<code> cp -r * /var/www/html </code>

3. Accedemos a la web utilizando http://mi_ip_publica

![13](https://github.com/Scosrom/practicas/assets/114906778/69bf33a8-d057-4b54-ac05-f88b5b73aa0d)
