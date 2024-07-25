# Terraform logging

## Enable logging

Terraform has detail logs which can be enabled by setting `TF_LOG` environment variable to any values.

Also we can set `TF_LOG_PATH` to setup path for logging file.

## Verbose logging terraform

Beside of enable logging, in order to decreasing verbosity we can set `TF_LOG` to one of the log levels:

- TRACE: enable showing internal logs of terraform which give us more details for debugging of `terraform plan` or `terraform apply` or `terraform destroy` step.
- DEBUG
- INFO
- WARN
- ERROR
