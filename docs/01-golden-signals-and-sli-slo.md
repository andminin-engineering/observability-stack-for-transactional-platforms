# 01-golden-signals-and-sli-slo

## Objetivo

Definir el framework de confiabilidad para servicios de pagos usando Golden Signals, SLI/SLO y politicas de error budget con enforcement operativo.

## Golden Signals adaptadas a Payments

| Signal | SLI propuesto | SLO objetivo Tier 1 | Formula de medicion | Accion ante incumplimiento |
|---|---|---|---|---|
| Latency | p95 de /payments/authorize y /payments/capture | p95 < 300 ms en ventana de 5 min | p95(latency_ms{endpoint=authorize|capture}) | Escalado preventivo, analisis de dependencias y freeze de cambios de alto riesgo |
| Traffic | TPS y tasa de autorizaciones por canal | Estabilidad dentro de banda esperada por hora comercial | sum(rate(payment_requests_total[1m])) | Activar capacity plan y ajuste de autoscaling por TPS |
| Errors | Ratio de 5xx y fallos tecnicos por proveedor | Error rate tecnico <= 0.10% y exito core >= 99.99% | rate(http_5xx_total)/rate(http_requests_total) | Incident response P1, rollback o circuit breaker segun impacto |
| Saturation | Uso de CPU/memoria en Fargate + conexiones Aurora | CPU p95 < 70%, memoria p95 < 75%, conexiones DB < 80% del max | p95(container_cpu), p95(container_memory), aurora_conn_utilization | Throttling controlado, scale-out y mitigacion de cuello de botella |

## SLI de negocio y confiabilidad transaccional

- Porcentaje de transacciones aprobadas tecnicamente (excluyendo rechazos de negocio).
- Tiempo end-to-end de autorizacion hasta confirmacion de persistencia.
- Ratio de timeout en integraciones externas por proveedor.
- Ratio de idempotency conflicts en operaciones de reintento.

## Politica de SLO y Error Budget

SLO critico de procesamiento core:
- Objetivo: 99.99% de exito tecnico mensual.
- Error budget mensual: 0.01% de fallos permitidos.

Ejemplo de calculo:
- Si el servicio procesa 100,000,000 transacciones/mes, el presupuesto de error permitido es 10,000 fallos tecnicos.

Modelo de gestion:
- Consumo <= 25%: operacion normal.
- Consumo 25%-50%: vigilancia reforzada, revision diaria.
- Consumo 50%-75%: congelar cambios no urgentes y reforzar quality gates.
- Consumo > 75%: Change Freeze obligatorio para releases no mitigatorios.
- Consumo >= 100%: solo se permite despliegue de remediacion/incidente.

## Relacionado

- [02 - Metrics and Alerting Strategy](02-metrics-and-alerting-strategy.md)
- [03 - Distributed Tracing and OpenTelemetry](03-distributed-tracing-and-opentelemetry.md)
- [05 - Observability Governance](05-observability-governance.md)
- Infraestructura base del portfolio: [aws-platform-reference](https://github.com/andminin-engineering/aws-platform-reference)
- Quality gates y delivery governance: [enterprise-cicd-governance-blueprint](https://github.com/andminin-engineering/enterprise-cicd-governance-blueprint)
