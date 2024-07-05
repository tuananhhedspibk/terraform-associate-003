# Terraform workspace

## What is workspace ?

To make it simple, you can understand it like "environment," for example, "develop," "staging," or "production."

They share the same code base, but the states are separate.

Upon initiation, the `default` workspace is automatically created. It's a permanent fixture and **CANNOT** be deleted.

## Commands with workspace

### terraform workspace new [name]

Create a new workspace.

### terraform workspace list

Listing all of the workspaces in your local.

### terraform workspace delete [name]

Delete workspace but pay attention that **we can not delete the default workspace**

## Using workspace in code

```tf
provider "aws" {
  region = var.region
}

resource "aws_instance" "example" {
  count         = terraform.workspace == "production" ? 5 : 1
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  tags = {
    Name = "example-${terraform.workspace}"
  }
}
```

Break down the example, we can use workspace in coding by `terraform.workspace`, with this we can check the current workspace when we execute terraform planning or terraform applying
