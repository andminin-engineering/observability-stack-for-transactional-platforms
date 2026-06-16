# observability-stack-for-transactional-platforms

Blueprint de observabilidad enterprise para plataformas de pagos distribuidas en AWS ECS Fargate, con OpenTelemetry, Prometheus, Grafana y Loki como stack de referencia operativa.

## Contexto operativo

Las APIs de pagos operan bajo alta sensibilidad de latencia, disponibilidad y trazabilidad regulatoria. Una degradacion de segundos en autorizaciones, capturas o conciliaciones impacta ingresos, reputacion y riesgo contractual con adquirentes/emisores.

Este repositorio define una estrategia integral para monitorear servicios transaccionales criticos, detectar anomalías antes de afectar al cliente y reducir MTTR mediante correlacion de metricas, trazas y logs.

## Desafios de observabilidad en Payments

- Trazabilidad distribuida extremo a extremo en microservicios Java Spring Boot y Node.js NestJS.
- Correlacion consistente entre logs estructurados, metrica de negocio y spans de trazas.
- Visibilidad sobre dependencias externas (bancos, gateways, antifraude) con SLA variable.
- Separacion entre ruido operativo y señales reales de riesgo para evitar alert fatigue.
- Cumplimiento PCI-DSS en telemetria, evitando exposicion de datos sensibles.

## Alcance

Incluye:
- Definicion de Golden Signals, SLI/SLO y gestion de error budget.
- Estrategia de metricas y alertas por severidad.
- Trazabilidad distribuida con OpenTelemetry en ECS Fargate.
- Politicas de logging estructurado y auditoria.
- Governance SRE de dashboards, ownership y capacity reviews.

No incluye en esta fase:
- Codigo de aplicacion.
- Dashboards productivos finales por dominio.
- Registros con datos reales de clientes.

## Indice del repositorio

- docs/01-golden-signals-and-sli-slo.md
- docs/02-metrics-and-alerting-strategy.md
- docs/03-distributed-tracing-and-opentelemetry.md
- docs/04-structured-logging-and-audit.md
- docs/05-observability-governance.md

## Resultado esperado

Un marco de confiabilidad medible, auditable y operable para plataformas de pagos de alta disponibilidad, alineado a practicas SRE enterprise.
