# Optimizacion de costos

## Acciones propuestas

1. **Rightsizing de EC2**: monitorear CPU real con el dashboard de
   CloudWatch durante 1-2 semanas; si el uso promedio es bajo, mantener
   t2.micro/t3.micro es correcto. Si se detecta sobreaprovisionamiento,
   considerar instancias tipo Graviton (t4g) que son mas economicas.

2. **S3 Intelligent-Tiering o ciclo de vida**: ya implementado (transicion
   a Standard-IA a los 30 dias) para medios y backups poco accedidos,
   reduciendo el costo de almacenamiento hasta un 40% en objetos frios.

3. **Horarios de apagado en entornos no productivos**: para ambientes de
   prueba/desarrollo (no aplica en produccion 24/7), usar Instance
   Scheduler o Lambda + EventBridge para apagar EC2 fuera de horario
   laboral, reduciendo hasta un 65% del costo de computo en esos entornos.

4. **Reserva de capacidad (Savings Plans / Reserved Instances)**: al
   confirmar que la carga es estable en produccion, migrar EC2 y RDS a
   Savings Plans de 1 año reduce el costo hasta un 30-40% frente a
   on-demand.

5. **Revisar Multi-AZ segun criticidad real**: Multi-AZ en RDS casi
   duplica el costo de base de datos; se mantiene aqui por el requisito de
   alta disponibilidad del caso de estudio, pero en cargas de trabajo no
   criticas podria evaluarse Single-AZ con snapshots frecuentes como
   alternativa mas economica.

## Comparacion arquitectura inicial vs optimizada

| Aspecto | Inicial (on-demand) | Optimizada |
|---|---|---|
| EC2 | t2.micro on-demand 24/7 | t4g.micro + Savings Plan 1 año |
| RDS | Multi-AZ on-demand | Multi-AZ con Reserved Instance |
| S3 | Standard todo el tiempo | Standard + transicion a IA a 30 dias |
| Costo mensual estimado | ~67.0 US$ | ~45-50 US$ (~25-30% de ahorro) |
