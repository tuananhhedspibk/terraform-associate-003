# for-each and dynamic block

The main reason that makes me want to combine this these two concepts is we can use `for-each` and `dynamic block` to create:

- Repeatable resources
- Looping

## for-each

For example with `for_each`

```tf
variable public_subnets {
  default = {
    "public_subnet_1" = 1
    "public_subnet_2" = 2
    "public_subnet_3" = 3
  }
}

resource "aws_subnet" "public_subnets" {
  for_each                = var.public_subnets // iterable set
  map_public_ip_on_launch = true
}
```

Breakdown of the example:

For **variable definition**, we have a map with three key/value pairs

- public_subnet_1 / 1
- public_subnet_2 / 2
- public_subnet_3 / 3

For **resource definition** `for_each = var.public_subnets` will tell to terraform that: "I want you to create three subnets correspond to the public_subnets config ('public_subnet_1', 'public_subnet_2' and 'public_subnet_3')" and as a result, we will have `3 subnets`:

- public_subnet_1
- public_subnet_2
- public_subnet_3

are created.

By the way, I want to talk about `count`.

Both `for_each` and `count` are used to create multiple instances at the same time, but:

- `for_each` can work with `set` or `map,` besides it provides a way to reference instances by `key`.
- `count` can work with `list` only.

Let me give you an example:

Firstly is with `for_each`

```tf
variable "security_groups" {
  type = map(object({
    name        = string
    description = string
  }))

  default = {
    sg1 = {
      name        = "allow_ssh"
      description = "Allow SSH traffic"
    }
    sg2 = {
      name        = "allow_http"
      description = "Allow HTTP traffic"
    }
  }
}

resource "aws_security_group" "example" {
  for_each    = var.security_groups

  name        = each.value.name
  description = each.value.description
  vpc_id      = aws_vpc.main.id

  ingress {
    from_port  = 22
    to_port    = 22
    protocol   = "tcp"
    cidr_block = ["0.0.0.0/0"]
  }

  egress {
    from_port  = 0
    to_port    = 0
    protocol   = "-1"
    cidr_block = ["0.0.0.0/0"]
  }
}
```

Breakdown of the example:

For **variable definition**, defines a variable `security_groups` which is a map of objects. Each object contains `name` and `description` attributes.

For **resource definition**, use `for_each` to iterate through the `security_groups` map. For each security_group, it dynamically assign the `name` and `description` based on the current `each.value`.

And now is `count`

```tf
variable "instances" {
  type = list(object({
    ami           = string
    instance_type = string
    name          = string
  }))

  default = [
    {
      ami           = "ami-0c55b159cbfafe1f0"
      instance_type = "t2.micro"
      name          = "example-instance-1"
    },
    {
      ami           = "ami-0c55b159cbfafe1f1"
      instance_type = "t2.small"
      name          = "example-instance-3"
    },
    {
      ami           = "ami-0c55b159cbfafe1f2"
      instance_type = "t2.medium"
      name          = "example-instance-3"
    }
  ]
}

resource "aws_instance" "example" {
  count         = length(var.instances)
  ami           = var.instances[count.index].ami
  instance_type = var.instances[count.index].instance_type

  tags = {
    Name = var.instances[count.index].name
  }
}
```

Breakdown of the example:

For **variable definition**, defines `instances` which is a list of objects. Each object contains `ami`, `instance_type`, `name` attributes.

For **resource definition**, use the `count` parameter set to the length of the `instances` list. For each instance, it dynamically assigns the `ami`, `instance_type`, `name` based on the current `count.index`.

## dynamic block

Let's move to dynamic block, we use dynamic block to create "blocks" (sometimes is nested block).

A dynamic block has the following information:

- `content`: iterable content.
- `for_each`: collection to iterate through.
- `iterator`: this is optional, we use it to indicate the name of the variable is used in `content` block.

Take an example

```tf
variable "ingress_rules" {
  type = list(object({
    from_port   = number
    to_port     = number
    protocol    = string
    cidr_blocks = list(string)
  }))

  default = [
    {
      from_port   = 80
      to_port     = 80
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    },
    {
      from_port   = 22
      to_port     = 22
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  ]
}

resource "aws_security_group" "example" {
  name   = "example"
  vpc_id = aws_vpc.main.id

  dynamic "ingress" {
    for_each = var.ingress_rules

    content {
      from_port   = ingress.value.from_port
      to_port     = ingress.value.to_port
      protocol    = ingress.value.protocol
      cidr_blocks = ingress.value.cidr_blocks
    }
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

Breakdown of the example:

For **variable definition**, defines the `ingress_rules` which is a list of object that contains `from_port`, `to_port`, `protocol`, `cidr_blocks` attributes.

For **resource definition**, **dynamic** key word will help us create blocks equal in number to the number of elements in the `ingress_rules` list and dynamically assign the `from_port`, `to_port`, `protocol`, `cidr_blocks` based on `ingress.value`.
