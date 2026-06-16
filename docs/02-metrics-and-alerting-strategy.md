# 02-metrics-and-alerting-strategy

## Objetivo

Diseñar una estrategia de metricas y alertas con Prometheus y Grafana para detectar degradaciones de plataforma y negocio en tiempo util para mitigacion.

## Recoleccion de metricas

Capas monitoreadas:
- Negocio: TPS, ratio de autorizacion, tasa de timeouts por proveedor, fraude tecnico.
- Aplicacion: latencia por endpoint, 4xx/5xx, pool de conexiones, colas internas.
- Plataforma: CPU/memoria por tarea Fargate, reinicios, saturacion de red.
- Datos: conexiones Aurora, replicas lag, IOPS, latencia de query.
- Integraciones: disponibilidad y latencia por proveedor externo.

## Estrategia de tableros Grafana

- Dashboard ejecutivo: disponibilidad, exito transaccional y consumo de error budget.
- Dashboard SRE: signals tecnicas, saturacion, dependencia externa, estado de alertas.
- Dashboard de dominio: KPIs por producto, proveedor y region.

## Matriz de alertas por severidad

| Severidad | Condicion | Threshold | Ventana | Runbook |
|---|---|---|---|---|
| Critical | Error rate tecnico core elevado | > 1.0% | for: 2m | RB-001-payment-gateway-degradation |
| Critical | Latencia p95 en autorizaciones | > 800 ms | for: 3m | RB-002-authorization-latency-spike |
| Critical | Saturacion de conexiones Aurora | > 85% | for: 2m | RB-003-aurora-connection-saturation |
| Warning | Error rate tecnico moderado | > 0.3% | for: 5m | RB-004-elevated-error-rate |
| Warning | Latencia p95 en captura | > 500 ms | for: 5m | RB-005-capture-latency-degradation |
| Warning | CPU p95 Fargate | > 75% | for: 10m | RB-006-fargate-cpu-pressure |
| Info | Drift de trafico vs baseline horario | > 20% | for: 15m | RB-007-traffic-anomaly-observation |

## Runbooks criticos (resumen)

### RB-001-payment-gateway-degradation
- Verificar proveedor dominante afectado y region impactada.
- Activar circuit breaker selectivo y fallback de proveedor si aplica.
- Coordinar con incident commander y actualizar estado publico interno.

### RB-003-aurora-connection-saturation
- Confirmar origen por servicio/endpoint.
- Aplicar mitigacion: ajuste de pool, limitacion temporal de trafico no critico.
- Escalar capacidad o failover de lectura segun comportamiento.

## Enlace con quality gates

- Alertas critical recientes bloquean promotion a prod salvo excepcion formal.
- Despliegues con degradacion de SLI clave pasan a aprobacion manual.
- Reporte de alertas de ultima ventana se adjunta como evidencia de release.

## Relacionado

- [01 - Golden Signals and SLI SLO](01-golden-signals-and-sli-slo.md)
- [03 - Distributed Tracing and OpenTelemetry](03-distributed-tracing-and-opentelemetry.md)
- [04 - Structured Logging and Audit](04-structured-logging-and-audit.md)
- [05 - Observability Governance](05-observability-governance.md)
- Infraestructura base del portfolio: [aws-platform-reference](https://github.com/andminin-engineering/aws-platform-reference)
- Quality gates y delivery governance: [enterprise-cicd-governance-blueprint](https://github.com/andminin-engineering/enterprise-cicd-governance-blueprint)
