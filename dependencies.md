# Dependencies

## Explicit dependencies

```tf
resource "aws_instance" "example" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  depends_on = [
    aws_security_group.example
  ]

  tags = {
    Name = "example-instance"
  }
}
```

Break down example, by using `depends_on` keyword we can see that `aws_instance` is being depent on `aws_security_group` in explicit way.

## Implicit dependencies

This is how Terraform creates a dependency graph from the config file based on referencing between resources.

```tf
resource "aws_security_group" "example" {
  name        = "example-sg"
  description = "Example security group"
  vpc_id      = var.vpc_id

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "example" {
  ami             = "ami-12345678"
  instance_type   = "t2.micro"
  security_groups = [
    aws_security_group.example.name
  ]

  tags = {
    Name = "example-instance"
  }
}
```

Break down the example, before creating `aws_instance` we must have `aws_security_group` so `security_group` will be created first. So `security_group` will be called `implicit dependency`.

Pros:

1. Simplify config file without `depends_on` keyword.
2. Ensure that terraform will create resources in the correct sequence.
