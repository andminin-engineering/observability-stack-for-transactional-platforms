# 03-distributed-tracing-and-opentelemetry

## Objetivo

Definir estrategia de trazabilidad distribuida en ECS Fargate con OpenTelemetry para seguir transacciones de pagos extremo a extremo y correlacionarlas con eventos de logs y metricas.

## Patron de despliegue OTel en ECS Fargate

Modelo recomendado:
- Un OpenTelemetry Collector como sidecar por tarea ECS para minimizar salto de red y aislar backpressure.
- Export de spans a backend de trazas central (segun stack elegido) y derivacion de metricas por spans.
- Enriquecimiento de recursos con metadatos: service.name, environment, region, ecs.task_id, payment_domain.

Ventajas del sidecar:
- Menor latencia de exportacion y mejor control de retry/batching.
- Politicas de sampling por servicio sin depender de componente externo compartido.
- Aislamiento de fallos de telemetria respecto al contenedor principal.

## Estrategia de sampling

Head-based sampling:
- Bajo costo y simplicidad operativa.
- Recomendado para trafico de alto volumen con baseline estable.

Tail-based sampling:
- Permite retener 100% de trazas con error, alta latencia o atributos criticos.
- Recomendado para pagos por su capacidad de preservar casos raros de falla.

Modelo hibrido sugerido:
- Head-based para trafico saludable (por ejemplo 5%-10%).
- Tail-based prioritario para errores, timeouts de proveedor, p95 fuera de SLO y transacciones de alto valor.

## W3C Trace Context y propagacion

- Cada request HTTP transporta traceparent y tracestate.
- El API gateway/edge inicia o continua el trace_id.
- Cada microservicio agrega spans hijos con span_id unico.
- La propagacion cruza CloudFront, ALB, ECS services, adapters de proveedor y acceso a base de datos.

## Log Correlation

- Cada log de aplicacion incluye trace_id y span_id.
- Grafana/Loki permite pivotar de alerta de metrica a log y a traza completa.
- Los dashboards deben ofrecer drill-down: KPI -> alerta -> trace -> logs correlacionados.

## Relacionado

- [01 - Golden Signals and SLI SLO](01-golden-signals-and-sli-slo.md)
- [02 - Metrics and Alerting Strategy](02-metrics-and-alerting-strategy.md)
- [04 - Structured Logging and Audit](04-structured-logging-and-audit.md)
- [05 - Observability Governance](05-observability-governance.md)
- Infraestructura base del portfolio: [aws-platform-reference](https://github.com/andminin-engineering/aws-platform-reference)
- Quality gates y delivery governance: [enterprise-cicd-governance-blueprint](https://github.com/andminin-engineering/enterprise-cicd-governance-blueprint)
