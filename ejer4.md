Para visualizar la estructura de los directorios sites-available y sites-enabled en Nginx, puedes usar el comando tree. Asegúrate de tener instalada la herramienta tree con:

```
sudo apt-get install tree
```

Luego, puedes ejecutar los siguientes comandos:

```
# Mostrar la estructura de sites-available
echo "Estructura de sites-available:"
tree /etc/nginx/sites-available
```
```
# Mostrar la estructura de sites-enabled
echo "Estructura de sites-enabled:"
tree /etc/nginx/sites-enabled
```

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/0c9df5b5-89ff-4b5f-bd69-0a8066f2afaf)
