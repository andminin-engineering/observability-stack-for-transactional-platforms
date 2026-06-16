# 04-structured-logging-and-audit

## Objetivo

Definir estandares empresariales de logging estructurado para operaciones de pagos, habilitando auditoria robusta, correlacion con trazas y cumplimiento PCI-DSS.

## Politica de logging estructurado JSON

Principios:
- Todo log en formato JSON valido y consistente entre servicios.
- Sin datos sensibles sin enmascarar.
- Correlacion obligatoria con telemetria distribuida.
- Separacion entre logs de negocio, operativos y seguridad.

Schema de referencia:

```json
{
  "timestamp": "2026-06-16T15:42:31.114Z",
  "level": "ERROR",
  "environment": "prod",
  "service_name": "payment-authorization-service",
  "trace_id": "4bf92f3577b34da6a3ce929d0e0e4736",
  "span_id": "00f067aa0ba902b7",
  "transaction_id": "txn_9c4e2d72f8",
  "payment_provider": "provider_x",
  "status_code": 502,
  "error_code": "PROVIDER_TIMEOUT",
  "message": "Authorization failed due to upstream timeout",
  "region": "sa-east-1"
}
```

## Politica de sanitizacion PCI-DSS

- Enmascarado obligatorio de PAN (solo primeros 6 y ultimos 4 si es estrictamente necesario).
- Prohibicion de registrar CVV, track data y secretos de autenticacion.
- Tokens solo en forma truncada/no reversible en logs de aplicacion.
- Scrub de payloads sensibles antes de persistencia en Loki/CloudWatch.
- Revisiones periodicas con reglas automatizadas de deteccion de PII/PCI.

## Retencion y auditoria

- Retencion diferenciada por tipo de log y requerimiento regulatorio.
- Inmutabilidad de logs de seguridad y acceso administrativo.
- Trazabilidad de consultas y exportaciones de logs con control de acceso.
- Evidencia de auditoria integrada a procesos de compliance.

## Relacionado

- [01 - Golden Signals and SLI SLO](01-golden-signals-and-sli-slo.md)
- [02 - Metrics and Alerting Strategy](02-metrics-and-alerting-strategy.md)
- [03 - Distributed Tracing and OpenTelemetry](03-distributed-tracing-and-opentelemetry.md)
- [05 - Observability Governance](05-observability-governance.md)
- Infraestructura base del portfolio: [aws-platform-reference](https://github.com/andminin-engineering/aws-platform-reference)
- Quality gates y delivery governance: [enterprise-cicd-governance-blueprint](https://github.com/andminin-engineering/enterprise-cicd-governance-blueprint)
