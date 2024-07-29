# Terraform registry

## What is terraform registry

According to hashicorp

> Terraform registry is an interactive resource for discovering a wide selection of integration (providers), configuration packages (modules) and security rules (policies) for use with terraform

The main aim of registry is to provide plugins to manage any infrastructure API.

## Requirement to publish terraform public registry

1. Must have correspond github public repository.
2. Repositorys name must have format `terraform-<PROVIDER>-<NAME>` - with `PROVIDER` can be aws or google, ... `NAME` is just a descriptive name for repository. For example: `terraform-aws-ec2-instance`
3. Module structure must be

```text
├── main.tf
├── variables.tf
├── outputs.tf
└── README.md
```

4. Document must have:

- Description
- Usage
- Inputs
- Outputs

5. Semantic versioning: for example `v1.0.0` or `v1.0.1`, you can make a tag version for github repo by the following commands

```sh
git tag v1.0.0
git push origin v1.0.0
```
