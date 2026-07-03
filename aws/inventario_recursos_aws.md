# Inventario de recursos AWS

Todos los recursos se crearon via AWS CLI desde AWS CloudShell, region
us-east-1.

## Red
| Recurso | ID |
|---|---|
| VPC | vpc-028d4d13cb2b1dea7 |
| Subred publica A (us-east-1a) | subnet-0ad793c21e131a015 |
| Subred publica B (us-east-1b) | subnet-038d3eb8363c0835e |
| Subred privada A (us-east-1a) | subnet-056efde97281781d2 |
| Subred privada B (us-east-1b) | subnet-0117b50be2af2f775 |
| Internet Gateway | igw-0fccfce471a41bb3f |
| Tabla de rutas publica | rtb-0f846fa5aa11757c4 |

## Seguridad
| Security Group | ID |
|---|---|
| alb-comercial-nova | sg-0d241de6720dc5cf3 |
| ec2-comercial-nova | sg-0720c92f4068f734d |
| rds-comercial-nova | sg-0cb5e05a9bf62d1b9 |

## Computo
| Recurso | ID |
|---|---|
| AMI usada (Amazon Linux 2023) | ami-051bfa33df3949860 |
| EC2 instancia 1 (AZ-a) | i-04cf06bba40dc6aa0 |
| EC2 instancia 2 (AZ-b) | i-0476be54788e92251 |
| Key pair | wordpress-key |

## Balanceo de carga
| Recurso | Valor |
|---|---|
| Target Group | arn:aws:elasticloadbalancing:us-east-1:038588299070:targetgroup/tg-wordpress/042906670a594a57 |
| Load Balancer | arn:aws:elasticloadbalancing:us-east-1:038588299070:loadbalancer/app/alb-wordpress-comercial-nova/b5648ce2584a345a |
| DNS publico (URL del sitio) | alb-wordpress-comercial-nova-1096128296.us-east-1.elb.amazonaws.com |
| Health check | matcher HTTP 200,302; path / |
| Stickiness | habilitado, cookie de balanceador, 3600s |

## Base de datos
| Recurso | Valor |
|---|---|
| Identificador RDS | wordpress-db-comercial-nova |
| Motor | MySQL 8.4.8 |
| Clase | db.t3.micro |
| Multi-AZ | Si |
| Backup automatico | 7 dias, ventana 03:00-04:00 |
| Acceso publico | No |

## Almacenamiento
| Recurso | Valor |
|---|---|
| Bucket S3 | comercial-nova-wordpress-media-038588299070 |
| Versionado | Habilitado |
| Acceso publico | Bloqueado |
| Ciclo de vida | Transicion a Standard-IA a los 30 dias |

## Comandos de despliegue (orden de ejecucion)
1. Creacion de VPC, subredes, Internet Gateway y tabla de rutas.
2. Creacion de 3 Security Groups (alb, ec2, rds) con reglas de minimo
   privilegio.
3. Creacion de RDS Subnet Group e instancia RDS MySQL Multi-AZ.
4. Lanzamiento de 2 instancias EC2 con

4. Lanzamiento de 2 instancias EC2 con script user-data (instalacion de
   Apache, PHP y WordPress).
5. Creacion de Target Group, registro de instancias, creacion de ALB y
   Listener HTTP.
6. Ajuste del health check del Target Group para aceptar codigos 200 y 302
   (comportamiento normal de WordPress antes de completar la instalacion).
7. Creacion de 2 alarmas de CloudWatch (CPU > 70%) y un dashboard con
   metricas de EC2 y RDS.
8. Creacion de bucket S3 con bloqueo de acceso publico, versionado y
   politica de ciclo de vida; prueba de carga y descarga de archivo.

Todos los comandos exactos ejecutados quedan documentados como historial
de esta entrega (ver capturas en evidencias/capturas_servicios/).
