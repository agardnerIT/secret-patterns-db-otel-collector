# secret-patterns-db-otel-collector
Takes mazen160/secrets-patterns-db and converts rules to OpenTelemetry Collector filter configurations.

## Usage

Create `file.log` and put some (fake) sensitive info into it. For example:

```
First log line AKIAIOSFODNN7EXAMPLE
```

Then start the collector
```
/path/to/collector --config=collector.out.yaml
```

You should NOT see the log line in the collector output.


