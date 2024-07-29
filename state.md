# State in terraform

This is a necessary requirement for terraform to function.

For more details you can see in: <https://developer.hashicorp.com/terraform/language/state/purpose>

## The benefits of state in terraform

### Mapping to the real world

State helps mapping the resources in your terraform config to the resources in real world.

Terraform expects that each remote object is bound to only one resource instance in the configuration.

After creating resources in the cloud environment, terraform will record resource identities in the state.

### Metadata

Terraform also tracks metadata of resources via state.

Terraform simply uses configuration to determine dependency order. This is especially important when we want to destroy resources (what resources must be destroyed first? What resources will be destroyed after that?).

Let's take an example of an AWS provider.

```tf
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"

  enable_dns_hostnames = true
  enable_dns_support   = true

  tags = {
    Name = "vpc"
  }
}

resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id

  tags = {
    Name = "interget-gateway"
  }
}
```

In the above example, I want to create `aws_vpc` and `aws_internet_gateway`, which will be attached to the vpc. To achieve that, the `vpc_id` attribute of the configuration of `internet_gatway` must be set to **the id of vpc**.

When scanning to create resources, Terraform will understand that it has to create internet_gateway **after** creating vpc.

You can read more detail codes here: <https://github.com/tuananhhedspibk/NewAnigram-Infrastructure/blob/main/terraform/network/main.tf>

### Performance

Terraform state will cache the attribute values to improve the performance, especially in large-scale infrastructure systems; resource querying will meet the following problems:

- Slow (It takes a lot of time to query every resource).
- Some cloud providers do not provide API for querying multiple resources simultaneously.
- Cloud provider API rate limiter.

So, caching information about the resources in the state can help us improve performance.
