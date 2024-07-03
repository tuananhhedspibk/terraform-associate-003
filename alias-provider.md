# Alias for terraform provider

With `alias` we can use multiple provider blocks with the same name.

For example:

```tf
provider "aws" {
  region = "us-west-1"
  alias  = "us_west_1"
}

provider "aws" {
  region = "us-east-1"
  alias  = "us_east_1"
}

resource "aws_instance" "web_us_west_1" {
  provider      = aws.us_west_1
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  tags = {
    Name = "web-us-west-1"
  }
}

resource "aws_instance" "web_us_east_1" {
  provider      = aws.us_east_1
  ami           = "ami-87654321"
  instance_type = "t2.micro"

  tags = {
    Name = "web-us-east-1"
  }
}
```

Breakdown of the example:

We use two `aws` providers with different regions, which are aliased with "us_west_1" and "us_east_1" in the corresponding region.

To use each of them, we must specify an `alias name`. For example, in the code above, we have `aws.us_east_1` and `aws.us_west_1`.
