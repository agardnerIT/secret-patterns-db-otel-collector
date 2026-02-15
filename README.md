# Secret Patterns DB - OpenTelemetry Collector

A collection of high-confidence secret detection rules for OpenTelemetry Collector.

## Background

This project generates OpenTelemetry Collector configurations that detect secrets in log files. Rules are derived from the [secrets-patterns-db](https://github.com/mazen160/secrets-patterns-db) project.

## Two Files, Two Scenarios

This project generates two output files:

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

### Generate the Configurations

```bash
# Generate collector.out.yaml (filter processor)
python download_rules.py

# Generate collector.transform.yaml (transform processor)
python generate_transform.py
```

### Run with Docker

```bash
# Filter processor
docker run -v $(pwd)/collector.out.yaml:/etc/otelcol-contrib/config.yaml otelcol-contrib --config=/etc/otelcol-contrib/config.yaml

# Transform processor
docker run -v $(pwd)/collector.transform.yaml:/etc/otelcol-contrib/config.yaml otelcol-contrib --config=/etc/otelcol-contrib/config.yaml
```

## Prerequisites

- Docker / Podman

## Metrics

When using the filter processor, metrics are available on port 8888:
- `otelcol_processor_filter_logs_filtered` - counter of filtered log records

## License

Apache

## AI Assisted
Parts of this repo were generated using [OpenCode](https://opencode.ai)
