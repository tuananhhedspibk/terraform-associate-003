# Terraform backend

## Overview

In terraform, backend is the config that indicates how state is loaded and persisted.

We have 3 main backend types:

- `local`: save as state file in local environment. This is the default type.
- `consul`: store state in the `Consul KV` (I haven't tried it, please try it yourself, lol).
- `s3`: store state in AWS S3.

## Why backend ?

1. `Collab`: prevent conflicting when multiple developers work on the same infrastructure environment.
2. `State locking`: prevent changing state simutaneosly.
3. `Remote storage`: ensure state file is stored in concentration, security storage.
4. `Backup and recovery`: for auto backup and recovery.

## Backend migration

We can migrate between backend types by using the below command:

```sh
terraform init -migrate-state
```

this command can help us reuse the providers those are loaded by reading `.terraform.lock.hcl` lock file

![Screenshot 2024-07-26 at 11 07 10](https://github.com/user-attachments/assets/486a60cb-ca6f-4112-b7e0-bde50c9ebab5)
