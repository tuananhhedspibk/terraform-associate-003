# Terraform cloud

## The relationship with terraform CLI

![Screenshot 2024-07-27 at 15 59 04](https://github.com/user-attachments/assets/3df57ba4-9de2-42c8-a0ad-7bd60a11e15c)

## Workspace

![Screenshot 2024-07-27 at 16 01 28](https://github.com/user-attachments/assets/e76edc66-87d9-4d86-a10f-7303193c0c90)

## Authentication

`terraform login` command will use `API Token` from `app.terraform.io` to login into terraform cloud.

## Version control

The main purpose is allowing multiple developers collaborate on the same terraform code.

Version control is integrated with `Github` or `Bitbucket`, ...

## Private registry

Storing and versioning terraform modules for reusing after.

Can link from Github (VCS) repositories. Those repositories name must be in format `terraform-<PROVIDER>-<NAME>`.

## Sentinel policy

It just likes `Policy-as-code`. Sentinel policy sits between `terraform plan` and `terraform apply` stages.

Policy has 3 main levels:

- `Advisory`: allow to be failed, warning log will be outputed.
- `Soft Mandatory`: allow overriding, without overriding its checking must be passed.
- `Hard Mandatory`: must pass and does not allow overriding.

Sentiny policy evaluation will be executed after terraform finish planning, cost estimating and before applying.

## Version control workflow

Used when collaborate in team. Linking terraform workspace with VCS repo and trigger applying or planning each time commiting code.

Multiple workspaces (ex: dev, stg, prod) can link and use the same code base from VCS repo.

For workspace that is linking with VCS, VCS-driven workflow must be ensured for `single source of truth` and now allow running `terraform apply` by CLI or another methods.

## Agents

Execute terraform plan and apply changes to your infrastructure.

Basic functions:

- Receive `terraform plan` output from terraform cloud.
- Run receiving plan at local environment.
- Apply those changes from local.
