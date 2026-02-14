# secret-patterns-db to OpenTelemetry Collector Filter Rules
Takes [mazen160/secrets-patterns-db/rules-stable.yml](https://github.com/mazen160/secrets-patterns-db/blob/master/db/rules-stable.yml) and converts the high confidence rules to OpenTelemetry Collector filter configurations.

There are two files depending on which scenario you want:
* Drop (filter out) telemetry (basic usage) ([collector.out.yaml](collector.out.yaml) uses the filter processor)
* Redact log lines and push redacted log into the backend system (advanced but allows for team attribution) ([collector.transform.yaml](collector.transform.yaml) uses the transform processor)


## Usage

Create `file.log` and put some (fake) sensitive info into it. For example:

```
First log line AKIAIOSFODNN7EXAMPLE
```

Then start the collector:

```
/path/to/collector --config=collector.out.yaml
```

or:

```
/path/to/collector --config=collector.transform.yaml
```


You should NOT see the log line in the collector output because the log line should be filtered out by the rule.

If you DO see the log line in the collector output, please open an issue here as the regex is wrong!

## Adding new rules

This repo should not get out of sync with the main rule list.

If you have new patterns, please contribute them to [mazen160/secrets-patterns-db](https://github.com/mazen160/secrets-patterns-db) and this repo be updated _from_ that repo.
