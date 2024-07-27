# Terraform useful commands

## terraform init

According to hashicorp:

> Initializes a working directory containing configuration files and install plugins for required providers.

Beside of that `terraform init` command has some options, for examples:

`terraform init -backend-config=PATH` - use to indicate backend config file.

or

`terraform init -backend-config="KEY=VALUE"` - specify config that has `KEY / VALUE` pair.

`terraform init` also caches the source code in local for downloaded plugins.

## terraform plan

According to hashicorp:

> Creates an execution plan, which lets you preview the changes that terraform will make to your infrastructure.

With this command, you can understand what resources will be created, updated, or destroyed if you apply those changes.

For example with creating new resource

![Screenshot 2024-07-27 at 11 05 41](https://github.com/user-attachments/assets/61a35914-3e85-4dfa-87ce-ece833a668e0)

With updating the existing resource

![Screenshot 2024-07-27 at 11 18 09](https://github.com/user-attachments/assets/75fccf37-4e17-49c8-8f5f-eed78bf455fb)

With the option `-out`, you can save the plan for latter use.

## terraform apply

According to hashicorp:

> Executes the actions proposed in a terraform plan to create, update or destroy infrastructure.

With above example about creating new `aws_vpc`, after executing `terraform apply` with answering my choice "yes" or "no" on creating resource:

![Screenshot 2024-07-27 at 11 11 48](https://github.com/user-attachments/assets/353ffcff-30bb-48eb-b8ae-c0c12f959dca)

I can have my own vpc on AWS cloud like this

![Screenshot 2024-07-27 at 11 16 36](https://github.com/user-attachments/assets/037530ce-b7db-4565-903d-61d1175b20bb)

Please pay attention that when running the apply command, even if some resources are rollbacked, successfully created resources will be deployed without rollback.

### -refesh option

With option `refresh=false` the command `terraform apply -refesh=false` will skip refreshing state step before applying the changes.

### -replace option

Use for focring re-create resource.

For example:

```tf
provider "aws" {
  region = "ap-northeast-1"
}

resource "aws_instance" "example" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  tags = {
    Name = "example-instance"
  }
}
```

You can run this to re-create the aws_instance.

```sh
terraform apply -replace=aws_instance.example
```

### Running with empty config file

When your config files (.tf) are empty, if you run `terraform apply`, all of the current existing resources will be destroyed for absolutely.

## terraform destroy

Like its name, you can use this command to destroy any your resources that you want

Type it and this is the notice from terraform:

![Screenshot 2024-07-27 at 11 18 35](https://github.com/user-attachments/assets/a9a7cc20-2695-46f4-b605-2212c35e36b2)

## terraform state

This is the group of commands for state management. You can type `terraform state` command to your terminal, all of the subcommands will be shown.

With my experience, I usually use `terraform state list` for listing resources in the state.

![Screenshot 2024-07-27 at 11 27 00](https://github.com/user-attachments/assets/2df2ee24-86ab-4f16-b2bd-3c366c686d30)

## terraform console

Launch interactive console of terraform.

This command reads terraform configuration from current working dir and terraform state file so CLI must have the ability to lock state for preventing state simultaneously modifying.

## terraform force-unlock

## terraform show

## terraform validate

## terraform refresh

## terraform import

## terraform output

## terraform get

## terraform fmt

## terraform graph
