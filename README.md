# Secret Patterns DB - OpenTelemetry Collector

A collection of high-confidence secret detection rules for OpenTelemetry Collector.

## Background

This project contains OpenTelemetry Collector configurations that detect secrets in log files. Rules are derived from the [secrets-patterns-db](https://github.com/mazen160/secrets-patterns-db) project.

## Two Files, Two Scenarios

This repo contains two output files:

### 1. collector.out.yaml (Filter Processor)

Uses the `filter` processor to drop logs that match secret patterns.

**Best for:** Teams who run their own collector instance.

**Why this approach?**
- You know exactly which files the redactions came from
- Use collector metrics (available on port 8888) to chart and alert on any redactions
- Simple drop-only approach - matched logs are discarded

### 2. collector.transform.yaml (Transform Processor)

Uses the `transform` processor to replace matched secrets with `REDACTED`.

**Best for:** SRE teams or central collector instances where it's not immediately obvious which team or file the redactions came from.

**Why this approach?**
- The `REDACTED` log can be safely pushed to an observability tool
- Metrics and alerts can be generated in the third-party tool itself
- Useful when you don't own the source of the logs

## Usage

## Prerequisites

- Docker / Podman

### Run with Docker

```bash
# Filter processor
docker run -p 8888:8888 -v $(pwd)/collector.out.yaml:/etc/otelcol-contrib/config.yaml \
  -v $(pwd)/test.log:/test.log \
  otel/opentelemetry-collector-contrib:latest --config=/etc/otelcol-contrib/config.yaml

# Transform processor
docker run -p 8888:8888 -v $(pwd)/collector.transform.yaml:/etc/otelcol-contrib/config.yaml \
  -v $(pwd)/test.log:/test.log \
  otel/opentelemetry-collector-contrib:latest --config=/etc/otelcol-contrib/config.yaml
```

## Metrics

When using the filter processor, metrics are available on port 8888 at the `/metrics` endpoint (ie. `http://localhost:8888/metrics`):
- `otelcol_processor_incoming_items_total` - number of log lines sent into the processor
- `otelcol_processor_filter_logs_filtered_total` - number of filtered log records
- `otelcol_processor_outgoing_items_total` - number of log lines being sent out of the processor

### Important Note

The `service.telemetry.metrics` section and the `-p 8888:8888` flag are **only needed for demo purposes**.

This configuration exposes metrics on all interfaces (`0.0.0.0`) so you can access them at `http://localhost:8888` from your host machine.

In a production setup:
1. Remove the `service.telemetry.metrics` block from the YAML
2. Remove the `-p 8888:8888` from the docker run command

The Prometheus scrape configuration inside the YAML will still work for self-scraping without these items (ie. the collector can scrape itself).

## License

Apache

## AI Assisted
Parts of this repo were generated using [OpenCode](https://opencode.ai)
