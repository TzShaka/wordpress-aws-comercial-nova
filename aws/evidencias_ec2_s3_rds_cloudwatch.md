# Evidencias de servicios AWS

Este documento enumera las capturas de pantalla incluidas en
`evidencias/capturas_servicios/` como evidencia de cada servicio.

## VPC
- Captura de la VPC y las 4 subredes (2 publicas, 2 privadas) en el mapa
  de recursos de la consola VPC.

## EC2
- Captura de las 2 instancias en estado "running".
- Captura del Target Group con ambas instancias en estado "healthy".
- Captura de la pantalla de instalacion de WordPress (asistente de 5
  minutos) accedida via la URL publica del ALB.
- Captura del panel de administracion de WordPress (wp-admin) ya con
  sesion iniciada.

## S3
- Captura del bucket creado con versionado y bloqueo de acceso publico
  visibles.
- Captura o log de consola mostrando la subida y descarga exitosa de un
  archivo de prueba (carpeta pruebas/).

## RDS
- Captura de la instancia RDS en estado "Available", motor MySQL, Multi-AZ
  = Si.
- Log de consola de la prueba de conexion desde EC2 (WordPress conectado
  correctamente a la base de datos, sitio funcional).

## CloudWatch
- Captura del dashboard "Dashboard-WordPress-ComercialNova" mostrando CPU
  de EC2 y RDS.
- Captura de las 2 alarmas ("CPU-Alta-EC2-1", "CPU-Alta-EC2-2") con su
  umbral (70%) visible.

## IAM
- Nota: se utilizo el rol LabRole provisto por AWS Academy Learner Lab
  (no fue posible crear roles IAM personalizados en este entorno). Ver
  justificacion en seguridad/matriz_accesos.md.
