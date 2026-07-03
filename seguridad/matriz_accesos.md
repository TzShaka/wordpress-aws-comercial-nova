# Matriz de accesos y seguridad

## Security Groups

| Security Group | Direccion | Puerto | Origen/Destino | Proposito |
|---|---|---|---|---|
| alb-comercial-nova | Ingress | 80 | 0.0.0.0/0 | Recibir trafico HTTP publico |
| ec2-comercial-nova | Ingress | 80 | SG del ALB | Solo el ALB puede hablarle a EC2 |
| ec2-comercial-nova | Ingress | 22 | IP del administrador (/32) | SSH restringido, no abierto a internet |
| rds-comercial-nova | Ingress | 3306 | SG de EC2 | Solo las EC2 pueden consultar la base de datos |

## Principio de minimo privilegio
- RDS no tiene IP publica (`--no-publicly-accessible`).
- El bucket S3 tiene bloqueado todo acceso publico
  (`BlockPublicAcls`, `IgnorePublicAcls`, `BlockPublicPolicy`,
  `RestrictPublicBuckets` = true).
- Las credenciales root de la cuenta no se usan para operaciones diarias;
  todo el despliegue se realizo con el rol LabRole asignado por AWS
  Academy al usuario del laboratorio.
- El acceso SSH a las instancias EC2 esta limitado a la IP publica del
  administrador, no abierto a internet.

## Cifrado
- Conexion RDS: TLS soportado por el motor MySQL administrado por AWS.
- S3: cifrado en reposo por defecto de Amazon S3 (SSE-S3).

## IAM
Se utilizo el rol `LabRole` provisto por AWS Academy Learner Lab, ya que
el entorno no permite la creacion de roles IAM personalizados. Este rol
tiene permisos preconfigurados para EC2, S3, RDS y CloudWatch, suficientes
para el despliegue. Se documenta como limitacion del entorno academico.
