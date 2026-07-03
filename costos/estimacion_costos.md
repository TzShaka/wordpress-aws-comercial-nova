# Estimacion de costos mensual

## Supuestos
Region: us-east-1. Uso continuo 24/7 durante 30 dias. Precios referenciales
de AWS bajo demanda (on-demand), sin descuentos por compromiso.

| Servicio | Configuracion | Costo mensual aprox. (US$) |
|---|---|---|
| EC2 (x2) | t2.micro, 730 hrs/mes c/u | ~16.6 |
| RDS MySQL | db.t3.micro, Multi-AZ, 20 GB | ~28.5 |
| Application Load Balancer | 1 ALB + LCU | ~16.4 |
| S3 | Standard, < 5 GB, pocas solicitudes | ~0.5 |
| CloudWatch | Dashboard + 2 alarmas | ~3.0 |
| Transferencia de datos | Estimado bajo trafico | ~2.0 |
| **Total estimado** | | **~67.0** |

Nota: en AWS Academy Learner Lab el consumo real esta cubierto por el
credito del laboratorio y no genera cargo directo al estudiante; esta
tabla representa el costo equivalente en una cuenta de produccion normal,
tal como pide la rubrica.

## Componentes de mayor costo
1. RDS Multi-AZ (~43% del total) — el mayor costo por duplicar la
   instancia de base de datos para alta disponibilidad.
2. Application Load Balancer (~24%) — costo fijo por hora mas cargo por
   LCU (Load Balancer Capacity Unit).
3. EC2 (~25%) — dos instancias corriendo continuamente.
