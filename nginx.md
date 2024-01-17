# Nginx

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/437ce162-f5f3-4f30-a56f-958b46dd4bb3)

## Introducción.

Es un servidor web de código abierto y software de proxy inverso. Su principal función es gestionar y servir solicitudes HTTP, así como también actuar como un servidor de correo electrónico para tareas específicas. Nginx es conocido por su rendimiento, escalabilidad y eficiencia, y es ampliamente utilizado para mejorar el manejo de tráfico web y equilibrar la carga en servidores.

Además de sus capacidades como servidor web, Nginx también se utiliza comúnmente como un proxy inverso para distribuir el tráfico a diferentes servidores, mejorar la seguridad, y proporcionar funciones avanzadas como la compresión de contenido y la caché. Su diseño modular y su capacidad para manejar múltiples conexiones simultáneas lo hacen una opción popular para aplicaciones web de alto rendimiento y sitios con tráfico intenso.

## Contenido

* 1- Comparativa con Apache
   
   * 1. [Comparativa](comparativa.md)

* 2- Instalación
   
   * 2.1- [Instalación](nginx-ins.md)

   * 2.1- [Configuración Básica](configba.md)
   
   * 2.2- [Balanceo de Carga](bcarga.md)
      
* 3- Casos practicos

* 3.1 Mapeado de Url

   * 3.1.1- [Ejercicio 1](ejer/ejer1.md)
   
   * 3.1.2- [Ejercicio 2](ejer/ejer2.md)
   
   * 3.1.3- [Ejercicio 3](ejer/ejer3.md)
   
   * 3.1.4- [Ejercicio 4](ejer/ejer4.md)
      
* 3.2 Autentificación, Autorización y Control de Acceso

   * 3.2.1- [Ejercicio 5](ejer/ejer5.md)
      
   * 3.2.2- [Ejercicio 6](ejer/ejer6.md)
      
   * 3.2.3- [Ejercicio 7](ejer/ejer7.md)

## Comparativa con Apache.

Tanto Nginx como Apache son servidores web populares, pero difieren en su arquitectura y enfoques.

**Nginx:**

1. **Diseño de concurrencia:** Está diseñado para ser ligero y eficiente, utiliza un enfoque de concurrencia no bloqueante que le permite manejar un gran número de conexiones simultáneas de manera eficiente.

2. **Rendimiento:** Es conocido por su rendimiento y es particularmente eficaz en situaciones de alto tráfico o con muchas conexiones concurrentes.

3. **Proxy inverso:** Destaca en el manejo de tareas de proxy inverso, balanceo de carga y servir como servidor de archivos estáticos.

**Apache:**

1. **Diseño de concurrencia:** Utiliza un modelo de concurrencia tradicional, asignando un hilo o proceso por cada conexión, lo que puede resultar menos eficiente en comparación con Nginx en ciertos escenarios.

2. **Módulos y extensibilidad:** Apache es conocido por su flexibilidad y extensibilidad a través de módulos, lo que lo hace ideal para una variedad de configuraciones y requisitos específicos.

3. **Historia:** Apache tiene una larga historia y ha sido un servidor web estándar durante muchos años, lo que lleva a una amplia base de usuarios y una abundancia de recursos.

En resumen, Nginx destaca en eficiencia y rendimiento, especialmente para servir contenido estático y actuar como proxy inverso, mientras que Apache es conocido por su versatilidad y extensibilidad a través de módulos, siendo una opción sólida para una variedad de configuraciones y necesidades. La elección entre ambos dependerá de los requisitos específicos del proyecto y las preferencias del usuario.


## - Referencias.
