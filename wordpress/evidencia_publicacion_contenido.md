# Evidencia de publicacion de contenido

Ver capturas en `evidencias/capturas_servicios/`:

1. Pantalla del asistente de instalacion de WordPress (seleccion de
   idioma) accedida via la URL publica del ALB.
2. Formulario de conexion a base de datos completado con los datos del
   endpoint RDS.
3. Formulario de informacion del sitio (titulo, usuario administrador,
   contrasena) previo a la instalacion final.
4. Pantalla de exito ("WordPress ha sido instalado").
5. Panel de administracion (wp-admin) con sesion iniciada como
   administrador.
6. Al menos una entrada (post) publicada desde el panel de administracion,
   visible tanto en el editor como en el sitio publico.

## Prueba de persistencia de contenido
El contenido publicado se almacena en la base de datos RDS compartida por
ambas instancias EC2, por lo que el contenido es identico sin importar cual
instancia atienda la solicitud (verificado con pruebas de conectividad
HTTP 200 en ambas instancias via SSH y localhost).
