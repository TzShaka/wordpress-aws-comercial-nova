# Monitoreo y alertamiento

## Dashboard
Nombre: `Dashboard-WordPress-ComercialNova`

Widgets configurados:
- CPUUtilization de las 2 instancias EC2 (i-04cf06bba40dc6aa0, i-0476be54788e92251)
- CPUUtilization de la instancia RDS (wordpress-db-comercial-nova)

Ver captura en `monitoreo/dashboard_metricas.png`.

## Alarmas

| Nombre | Metrica | Umbral | Evaluacion | Recurso |
|---|---|---|---|---|
| CPU-Alta-EC2-1 | CPUUtilization | > 70% | 2 periodos de 5 min | i-04cf06bba40dc6aa0 |
| CPU-Alta-EC2-2 | CPUUtilization | > 70% | 2 periodos de 5 min | i-0476be54788e92251 |

Estas alarmas notifican cuando la carga de CPU de una instancia se
mantiene elevada por al menos 10 minutos consecutivos, lo que indicaria
la necesidad de escalar horizontalmente (agregar mas instancias al Target
Group) o investigar un problema de rendimiento.

## Justificacion de metricas elegidas
Se eligio CPU como metrica principal porque es el indicador mas directo de
saturacion en instancias t2.micro/db.t3.micro (recursos limitados del
laboratorio), y porque tanto EC2 como RDS la exponen de forma nativa sin
configuracion adicional de agentes.
