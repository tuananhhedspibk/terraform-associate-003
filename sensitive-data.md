# Sensitive data

Including:

- Key.
- Database password.
- ...

We can use remote backend like `S3` for encrypting state and sensitive information.

State best practice:

1. Consider state like `sensitive data`.
2. Encrypt backend state.
3. Control access to state file.

## State is stored as plain text

Even with `sensitive` information, `terraform run` still prints those "sensitive data" into log.

## Terraform vault

Is the provider for managing and protecting sensitive data.

```tf
provider "vault" {
  address = "https://vault.example.com:8200"

  token = "s.your-vault-token"
}

resource "vault_generic_secret" "example" {
  path = "secret/data/myapp/config"

  data_json = jsondecode({
    username = "admin"
    password = "s3cr3t"
  })
}

data "vault_generic_secret" "example" {
  path = "secret/data/myapp/config"
}

output "username" {
  value = data.vault_generic_secret.example.data["secret/data/myapp/config"]
}
```

For:

- Vault tokens
- Vault role IDs
- Vault secret IDs

we should save them as enviroment variables.

```sh
export VAULT_ADDR='https://vault.example.com:8200'
export VAULT_TOKEN='s.your-vault-token'
```

Using vault has one weakness is `secrets` key and token will be stored in `state file`, so you have to protect this `state file`.
