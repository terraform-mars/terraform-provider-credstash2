---
page_title: "Provider: credstash2"
description: |-
  Terraform provider for reading secrets stored with credstash.
---

# credstash2 Provider

This provider is a fork of `terraform-mars/credstash` to resolve registry publishing issues with the original provider.

Use this provider to read secrets stored with [credstash](https://github.com/fugue/credstash), a utility for managing credentials in AWS using DynamoDB and KMS.

## Migrating from terraform-mars/credstash

If you were using the original `terraform-mars/credstash` provider, update your configuration:

```hcl
terraform {
  required_providers {
    credstash = {
      source  = "terraform-mars/credstash2"
      version = "0.5.1"
    }
  }
}
```

## Example Usage

```hcl
provider "credstash" {
  region = "us-east-1"
  table  = "credential-store"
}

data "credstash_secret" "my_secret" {
  name = "my-secret-name"
}

output "secret_value" {
  value     = data.credstash_secret.my_secret.value
  sensitive = true
}
```

## Authentication

The provider uses standard AWS authentication methods. Configure via:
- Environment variables (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`)
- Shared credentials file (`~/.aws/credentials`)
- IAM role (when running on AWS)

## Argument Reference

- `region` - (Optional) AWS region. Defaults to `us-east-1`.
- `table` - (Optional) DynamoDB table name. Defaults to `credential-store`.
- `profile` - (Optional) AWS profile name.
