# WordPress en AWS - Comercial Nova

## Integrantes y roles
- Howard Lemuel Coila Alberto — Arquitectura, despliegue e infraestructura

## Problema planteado y alcance
Comercial Nova necesita un portal web corporativo en WordPress con alta
disponibilidad, seguridad, escalabilidad y control de costos, desplegado
sobre AWS.

## Arquitectura propuesta
VPC propia (10.0.0.0/16) con 2 subredes públicas y 2 privadas en distintas
zonas de disponibilidad (us-east-1a / us-east-1b). Un Application Load
Balancer distribuye tráfico HTTP entre 2 instancias EC2 con WordPress. Los
datos se almacenan en una instancia RDS MySQL Multi-AZ. Los medios y backups
se guardan en S3. CloudWatch centraliza métricas y alarmas.

Ver diagrama completo en `arquitectura/diagrama.png` y el detalle de
decisiones en `arquitectura/justificacion_arquitectura.md`.

## Servicios cloud utilizados y por qué
| Servicio | Uso |
|---|---|
| VPC | Aislamiento de red y segmentación pública/privada |
| EC2 (x2) | Capa de aplicación WordPress (Apache + PHP) |
| Application Load Balancer | Distribución de tráfico y alta disponibilidad |
| RDS MySQL (Multi-AZ) | Base de datos administrada con failover automático |
| S3 | Almacenamiento de medios/backups, versionado activo |
| CloudWatch | Monitoreo de CPU en EC2/RDS y alertamiento |
| IAM (LabRole) | Rol de mínimo privilegio disponible en AWS Academy |

## Pasos para desplegar o reproducir
Ver comandos AWS CLI completos y en orden en `aws/inventario_recursos_aws.md`.
Resumen: VPC y redes -> Security Groups -> RDS -> EC2 (con user-data) ->
Target Group + ALB -> CloudWatch (dashboard + alarmas) -> S3.

## URL de acceso al WordPress desplegado
http://alb-wordpress-comercial-nova-1096128296.us-east-1.elb.amazonaws.com

## Evidencias de funcionamiento
Ver `evidencias/pruebas_funcionamiento.md` y `wordpress/evidencia_publicacion_contenido.md`.

## Estrategia de seguridad, monitoreo y costos
- Seguridad: `seguridad/matriz_accesos.md`
- Monitoreo: `monitoreo/alertas_configuradas.md`
- Costos: `costos/estimacion_costos.md` y `costos/optimizacion_costos.md`

## Evidencias especificas de AWS
Ver `aws/evidencias_ec2_s3_rds_cloudwatch.md` (VPC, EC2, S3, RDS, IAM, CloudWatch).

## Limitaciones conocidas y mejoras futuras
- AWS Academy Learner Lab no permite crear roles IAM personalizados ni NAT
  Gateway sin restricciones de presupuesto; se uso el rol LabRole
  provisto y las instancias EC2 se ubicaron en subredes publicas protegidas
  por Security Groups estrictos (SSH solo desde IP propia, HTTP solo desde
  el Security Group del ALB).
- El laboratorio del curso expiro una vez durante el desarrollo
  ("This course has ended and no longer accessible"), lo que retraso el
  despliegue; se documenta como incidente del entorno academico, no del
  diseno de la solucion.
- Mejora futura: mover EC2 a subredes privadas con NAT Gateway o VPC
  endpoints cuando el entorno lo permita; agregar HTTPS con ACM/Certificate
  Manager; integrar un plugin de offload de medios a S3.

## Lecciones aprendidas
- Los Security Groups no pueden nombrarse con el prefijo sg- (reservado
  para IDs de AWS).
- Los health checks del ALB deben ajustarse al comportamiento real de la
  aplicacion (WordPress sin instalar responde 302, no 200).
- Con discos EC2 independientes, cada instancia genera su propio
  wp-config.php; ambas apuntan a la misma base de datos RDS, por lo que
  el contenido real del sitio es consistente aunque los archivos de
  configuracion difieran (claves de seguridad aleatorias por instalacion).
