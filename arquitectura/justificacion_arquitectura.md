# Justificacion de arquitectura

## Red
Se creo una VPC propia (10.0.0.0/16) segmentada en 2 subredes publicas
(10.0.1.0/24 y 10.0.2.0/24) y 2 subredes privadas (10.0.3.0/24 y
10.0.4.0/24), distribuidas en dos zonas de disponibilidad (us-east-1a y
us-east-1b) para tolerancia a fallos de zona.

## Computo
Se desplegaron 2 instancias EC2 (t2.micro) con Amazon Linux 2023, cada una
ejecutando Apache + PHP + WordPress mediante un script de arranque
(user-data). Ambas quedan registradas en un Target Group detras de un
Application Load Balancer, que distribuye el trafico HTTP entrante.

Adaptacion sobre el diseno original: por restriccion de AWS Academy (sin
NAT Gateway disponible), las EC2 se ubicaron en subredes publicas en vez de
privadas, compensando el riesgo con Security Groups estrictos (ver
seguridad/matriz_accesos.md). Esta es una practica aceptada cuando el
control de acceso de red se mantiene a nivel de Security Group.

## Base de datos
Amazon RDS MySQL en modo Multi-AZ, ubicada en las subredes privadas y sin
acceso publico. El failover automatico ante caida de la zona primaria
provee alta disponibilidad de la capa de datos. Backups automaticos
habilitados con retencion de 7 dias, ventana 03:00-04:00 UTC.

## Alta disponibilidad y escalabilidad
- 2 instancias EC2 en distintas AZ detras de un ALB con health checks.
- RDS Multi-AZ con failover automatico.
- El Target Group solo enruta trafico a instancias en estado "healthy",
  por lo que una falla de una instancia no afecta la disponibilidad del
  sitio.
- Escalabilidad horizontal: se puede aumentar el numero de instancias EC2
  agregandolas al Target Group sin cambios de arquitectura.

## Almacenamiento
S3 para medios y backups, con acceso publico bloqueado, versionado activo
y politica de ciclo de vida que mueve objetos a Standard-IA tras 30 dias
para reducir costos.

## Seguridad
Principio de minimo privilegio aplicado mediante 3 Security Groups en
cadena: internet -> ALB (puerto 80) -> EC2 (puerto 80 solo desde el SG del
ALB, SSH puerto 22 solo desde la IP del administrador) -> RDS (puerto 3306
solo desde el SG de EC2). RDS no es publicamente accesible. Se utiliza el
rol LabRole provisto por AWS Academy por restriccion del entorno academico
(no permite crear roles IAM personalizados).

## Monitoreo
CloudWatch centraliza metricas de CPU de EC2 y RDS en un dashboard, con
alarmas configuradas para CPU > 70% en ambas instancias EC2.
