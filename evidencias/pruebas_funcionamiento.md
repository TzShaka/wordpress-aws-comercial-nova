# Pruebas de funcionamiento

## Resumen de validaciones realizadas

| Prueba | Resultado |
|---|---|
| VPC y subredes creadas en 2 AZ | OK |
| Security Groups con reglas de minimo privilegio | OK |
| RDS MySQL Multi-AZ en estado Available | OK |
| Instancias EC2 (x2) en estado running | OK |
| Target Group con ambas instancias en estado healthy | OK |
| ALB respondiendo en la URL publica | OK |
| Asistente de instalacion de WordPress accesible | OK |
| Conexion de WordPress a RDS exitosa | OK |
| Instalacion de WordPress completada | OK |
| Inicio de sesion en panel de administracion (wp-admin) | OK |
| Publicacion de al menos una entrada de contenido | OK |
| Ambas instancias EC2 sirviendo el mismo contenido (verificado via curl localhost, HTTP 200 en ambas) | OK |
| Carga y descarga de archivo de prueba en S3 | OK |
| Dashboard de CloudWatch con metricas de EC2 y RDS | OK |
| Alarmas de CPU configuradas (umbral 70%) | OK |

## URL final de acceso
alb-wordpress-comercial-nova-1096128296.us-east-1.elb.amazonaws.com

http://alb-wordpress-comercial-nova-1096128296.us-east-1.elb.amazonaws.com

## Incidentes durante el despliegue y su resolucion
1. Nombres de Security Group con prefijo "sg-" rechazados por AWS
   (reservado para IDs) -> se renombraron sin ese prefijo.
2. Version especifica de MySQL 8.0.35 no disponible en el laboratorio ->
   se omitio el parametro de version, dejando que RDS asignara la version
   estable mas reciente disponible (8.4.8).
3. Health check del ALB marcaba las instancias como "unhealthy" porque
   WordPress responde 302 antes de completar su instalacion -> se ajusto
   el matcher del Target Group para aceptar 200 y 302.
4. Balanceo alterno entre instancias durante el asistente de instalacion
   causo el error "wp-config.php already exists" -> se habilito
   stickiness de sesion en el Target Group.
5. El laboratorio de AWS Academy expiro una vez durante el desarrollo
   ("This course has ended and no longer accessible") -> se resolvio
   contactando al docente para su reactivacion.
