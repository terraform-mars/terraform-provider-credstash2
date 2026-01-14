---
page_title: "credstash_secret Data Source"
description: |-
  Reads a secret from credstash.
---

# credstash_secret

Reads a secret stored in credstash (DynamoDB + KMS).

## Example Usage

```hcl
data "credstash_secret" "db_password" {
  name = "production/db/password"
}

resource "aws_db_instance" "example" {
  password = data.credstash_secret.db_password.value
  # ...
}
```

### With Version

```hcl
data "credstash_secret" "api_key" {
  name    = "api-key"
  version = "0000000000000000003"
}
```

### With Context

```hcl
data "credstash_secret" "app_secret" {
  name = "app-secret"
  context = {
    environment = "production"
    app         = "myapp"
  }
}
```

## Argument Reference

- `name` - (Required) The name of the secret to retrieve.
- `version` - (Optional) Specific version of the secret. Defaults to latest.
- `context` - (Optional) Map of encryption context key-value pairs.

## Attribute Reference

- `value` - The decrypted secret value.
