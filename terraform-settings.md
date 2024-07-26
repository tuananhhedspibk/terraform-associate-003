# Terraform settings

## What is the aim?

For defining specific settings or behaviors of terraform itself such as requiring minimum version of terraform.

```tf
terraform {
  required_version ">= 0.12.0"

  backend "s3" {
    bucket = "my-terraform-state"
    key    = "terraform.tfstate"
    region = "ap-northeast-1"
  }

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "3.57.0"
    }
  }
}
```

`required_providers` is used to indicate the specific version of the provider that we want to use.
