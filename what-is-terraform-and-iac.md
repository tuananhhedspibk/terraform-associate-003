# What is terraform and IaC ?

## Terraform

According to Hashicorp definition

> Terraform is an infrastructure as code tool that lets you build, change, and version cloud and on-prem resources safely and efficiently.

Putting it simply, terraform is a tool that allows you to code with infrastructure instead of manual settings.

## IaC

IaC is the short form of `Infrastructure as Code`.

According to AWS

> Infrastructure as code (IaC) is the ability to provision and support your computing infrastructure using code instead of manual processes and settings.

About the benefit, IaC can give you something like:

- Versioning for infrastructure.
- Sharing and Reusing infrastructure code.

Besides that IaC can make the changes:

- Idempotent
- Consistent
- Repeatable
- Predictable

Without IaC, scaling up infrastructure will require more requirements, such as:

- Remotely connect to servers
- Manual provision and configuring server via a bunch of scripts or commands
- ...

## Problems when managing infrastructure manually
