# State in terraform

This is a necessaray requirement for terraform to function.

For more details you can see in: <https://developer.hashicorp.com/terraform/language/state/purpose>

The following is benefit of state in terraform

## Mapping to the real world

State helps mapping the resources in your terraform config to the resources in real world.

Terraform expects that each remote object is bound to only one resource instance in the configuration.
