# Terraform provision

## Overview

By default, the `terraform apply` command can provision a maximum of **10 resources** (the provisioning process will be executed in parallel) - which means that terraform can create, update, and destroy 10 resources simultaneously.

You can set the number of provision resources through `-parallelism` flag. This flat is usually modified when:

- Deploying large scale system.
- CI/CD pipeline (for strict depoy time limit).
- Reputing API provider Rate limit

## remote-exec & local-exec provisoner

We have two provisioners:

- remote-exec
- local-exec

with that we can run scripts or commands on remote or local environment.

We usually use it for server bootstraping.

Take an example with `remote-exec` - for remotely execute init commands.

```tf
resource "aws_instance" "example" {
  ami = "ami-1204312391239"
  instance_type = "t2.micro"

  provisioner "remote-exec" {
    connection {
      type = "ssh"
      user = "ec2-user"
      private_key = file("~/.ssh/my-key.pem")
    }

    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx",
      "sodu service nginx start"
    ]
  }
}
```

With `local-exec`

```tf
resource "aws_s3_bucket" "example" {
  bucket = "my-bucket"

  provisioner "local-exec" {
    command = "echo 'S3 bucket created!' > /tmp/s3_notification.txt"
  }
}
```
