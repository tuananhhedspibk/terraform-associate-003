# Environment variable

## Overview

Usually be defined in the file that has name `terraform.tfvar`.

For example:

```tf
AWS_REGION = "ap-northeast-1"
AWS_ACCESS_KEY = "key"
```

File `terraform.tfvars` must be listed in `.gitignore`

## TF_VAR

This is the prefix string for setting up input variables by using environment variable.

```sh
# export environment variable
export TF_VAR_instructor_name = "Foo"

terraform apply
```
