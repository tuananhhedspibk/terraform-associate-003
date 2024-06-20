# for-each and dynamic block

The main reason that makes me want to combine this these two concepts is we can use `for-each` and `dynamic block` to create:

- Repeatable resources
- Looping

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

Breakdown the example, `for_each = var.public_subnets` will tell to terraform that: "I want you to create three subnets correspond to the public_subnets config ('public_subnet_1', 'public_subnet_2' and 'public_subnet_3')" and as a result, we will have `3 subnets`:

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
