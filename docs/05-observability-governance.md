# 05-observability-governance

## Objetivo

Definir el operating model SRE para gobernar observabilidad de plataforma, ownership de tableros y mejora continua de capacidad.

## Operating model de ownership

Roles clave:
- Platform SRE Team: owner tecnico de stack de observabilidad, estandares y disponibilidad de tooling.
- Domain Tech Leads: owners de SLI/SLO por servicio y precision semantica de metricas.
- Incident Commander: autoridad operativa durante incidentes P1/P2.
- Architecture Board: gobierno de politicas transversales y excepciones.

Responsabilidades:
- Definir y mantener baseline de dashboards por dominio.
- Verificar que todo servicio Tier 1 tenga SLO y alertas critical activas.
- Revisar consumo de error budget y decidir freeze de cambios cuando aplique.

## Dashboard-as-Code

- Dashboards gestionados como codigo usando Grafana Terraform Provider.
- Versionado en repositorio, revision por pull request y trazabilidad de cambios.
- Promotion por ambientes con controles de validacion previos al apply.
- Politica de rollback para cambios de visualizacion o alerting.

## Capacity Planning semanal

Cadencia:
- Revision semanal de tendencia de TPS, latencia p95, saturacion y costos de telemetria.
- Proyeccion de capacidad para 4-8 semanas segun calendario comercial.
- Acciones preventivas antes de eventos de alto volumen.

Entregables minimos:
- Reporte de riesgo de capacidad por servicio Tier 1.
- Backlog de optimizaciones priorizado por impacto.
- Decision log de ajustes de umbral y planes de escalado.

## Governance de calidad de observabilidad

- Ningun servicio Tier 1 se considera production-ready sin dashboards y alertas validados.
- Cambios de umbral requieren evidencia historica y aprobacion tecnica.
- Postmortems obligatorios alimentan backlog de telemetria y runbooks.

## Relacionado

- [01 - Golden Signals and SLI SLO](01-golden-signals-and-sli-slo.md)
- [02 - Metrics and Alerting Strategy](02-metrics-and-alerting-strategy.md)
- [03 - Distributed Tracing and OpenTelemetry](03-distributed-tracing-and-opentelemetry.md)
- [04 - Structured Logging and Audit](04-structured-logging-and-audit.md)
- Infraestructura base del portfolio: [aws-platform-reference](https://github.com/andminin-engineering/aws-platform-reference)
- Quality gates y delivery governance: [enterprise-cicd-governance-blueprint](https://github.com/andminin-engineering/enterprise-cicd-governance-blueprint)
