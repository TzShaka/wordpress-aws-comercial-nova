# Instalacion y configuracion de WordPress

## Metodo de instalacion
WordPress se instalo mediante un script de arranque (user-data) ejecutado
automaticamente por cada instancia EC2 al lanzarse. El script:
1. Actualiza el sistema e instala Apache (httpd), PHP y las extensiones
   necesarias (php-mysqlnd, php-fpm, php-json).
2. Descarga la ultima version estable de WordPress desde wordpress.org.
3. Extrae los archivos en /var/www/html y ajusta permisos al usuario
   apache.
4. Inicia y habilita el servicio httpd.

Ver el script completo en infraestructura/scripts/user-data-wordpress.sh.

## Configuracion de base de datos
Al acceder por primera vez a la URL publica del ALB, WordPress presenta el
asistente de instalacion. Se configuro con los siguientes parametros:

- Host de base de datos: endpoint de RDS (wordpress-db-comercial-nova)
- Nombre de base de datos: wordpressdb
- Usuario: admin
- Prefijo de tablas: wp_ (por defecto)

## Incidencia y resolucion durante la instalacion
Al haber 2 instancias EC2 detras del ALB sin sesion pegajosa (stickiness),
el balanceo alterno las solicitudes entre ambas instancias durante el
asistente de instalacion, generando el error "The file wp-config.php
already exists". Se resolvio habilitando stickiness basado en cookie en el
Target Group (duracion 3600s), lo que garantiza que un mismo visitante
permanezca en la misma instancia durante su sesion.

## Ajuste del health check del Target Group
WordPress, antes de completar la instalacion, responde con codigo HTTP 302
(redireccion a wp-admin/setup-config.php) en vez de 200. El health check
del Target Group se ajusto para aceptar ambos codigos (200 y 302),
evitando falsos negativos de "unhealthy" durante el periodo de
instalacion inicial.
