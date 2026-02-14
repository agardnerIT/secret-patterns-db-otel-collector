# secret-patterns-db to OpenTelemetry Collector Filter Rules
Takes [mazen160/secrets-patterns-db/rules-stable.yml](https://github.com/mazen160/secrets-patterns-db/blob/master/db/rules-stable.yml) and converts the high confidence rules to OpenTelemetry Collector filter configurations.

## Usage

Create `file.log` and put some (fake) sensitive info into it. For example:

```
First log line AKIAIOSFODNN7EXAMPLE
```

Then start the collector
```
/path/to/collector --config=collector.out.yaml
```

You should NOT see the log line in the collector output because the log line should be filtered out by the rule.

If you DO see the log line in the collector output, please open an issue here as the regex is wrong!

## Adding new rules

This repo should not get out of sync with the main rule list.

If you have new patterns, please contribute them to [mazen160/secrets-patterns-db](https://github.com/mazen160/secrets-patterns-db) and this repo be updated _from_ that repo.
