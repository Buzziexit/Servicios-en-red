# Nginx

![image](https://github.com/Scosrom/Servicios-en-red/assets/114906778/437ce162-f5f3-4f30-a56f-958b46dd4bb3)

## Introducción.

Es un servidor web de código abierto y software de proxy inverso. Su principal función es gestionar y servir solicitudes HTTP, así como también actuar como un servidor de correo electrónico para tareas específicas. Nginx es conocido por su rendimiento, escalabilidad y eficiencia, y es ampliamente utilizado para mejorar el manejo de tráfico web y equilibrar la carga en servidores.

Además de sus capacidades como servidor web, Nginx también se utiliza comúnmente como un proxy inverso para distribuir el tráfico a diferentes servidores, mejorar la seguridad, y proporcionar funciones avanzadas como la compresión de contenido y la caché. Su diseño modular y su capacidad para manejar múltiples conexiones simultáneas lo hacen una opción popular para aplicaciones web de alto rendimiento y sitios con tráfico intenso.

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

## 3.- Instalación.
[Instalación](nginx-ins.md)

## 4.- Casos prácticos.

### Configurar Bloques de un servidor



5.- Referencias.
