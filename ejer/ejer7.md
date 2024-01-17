Para hacer esto, puedes usar la directiva allow en combinación con la directiva satisfy para controlar el acceso basado en la dirección IP. Aquí está cómo puedes hacerlo:

Supongamos que tienes la siguiente configuración de autenticación básica en tu archivo de configuración de Nginx:

```
server {
    listen 80;
    server_name tu_sitio;

    root /ruta/a/tu/sitio;
    index index.html;

    location / {
        try_files $uri $uri/ =404;

        # Configuración para la autenticación básica
        auth_basic "Área Restringida";
        auth_basic_user_file /etc/nginx/.htpasswd;

        # Configuración para permitir una IP sin autenticación básica
        allow ip_permitida;
        deny all;
        satisfy any;
    }
}
```

Reemplaza ip_permitida con la dirección IP que deseas permitir sin autenticación básica.

En este ejemplo, la directiva allow especifica la IP permitida, y deny all niega el acceso al resto. La directiva satisfy any indica a Nginx que se debe satisfacer al menos una condición (autenticación básica o dirección IP permitida) para permitir el acceso.

Si deseas que varias IP tengan acceso sin autenticación básica, puedes usar una lista separada por espacios en la directiva allow, por ejemplo:

```
allow ip1 ip2 ip3;
```

Después de realizar estos cambios, guarda el archivo y reinicia Nginx:

```
sudo systemctl restart nginx
```
Esto permitirá que la IP especificada acceda sin requerir autenticación básica, mientras que otras IPs seguirán necesitando autenticación.


Comprobación: 

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/dc908b23-4725-4359-a73f-205a4893541e)
