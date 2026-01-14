# Terraform provider for credstash secrets

> **Note:** This provider (`terraform-mars/credstash2`) replaces the original `terraform-mars/credstash` provider, which had registry sync issues due to a repository migration. If you were using `terraform-mars/credstash`, please migrate to this provider.

Read secrets stored with [credstash][credstash].

## Install

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

## Usage

```hcl
provider "credstash" {
    table  = "credential-store"
    region = "us-east-1"
}

data "credstash_secret" "rds_password" {
    name = "rds_password"
}

data "credstash_secret" "my_secret" {
    name    = "some_secret"
    version = "0000000000000000001"
}

resource "aws_db_instance" "postgres" {
    password = data.credstash_secret.rds_password.value

    # other important attributes
}
```

You can override the table on a per data source basis:

```hcl
data "credstash_secret" "my_secret" {
    table   = "some_table"
    name    = "some_secret"
    version = "0000000000000000001"
}
```

## AWS credentials

AWS credentials are not directly set. Use one of the methods discussed
[here][awscred].

You can set a specific profile to use:

```hcl
provider "credstash" {
    region  = "us-east-1"
    profile = "my-profile"
}
```

## License

MIT - See [LICENSE](LICENSE) file.

[credstash]: https://github.com/fugue/credstash
[awscred]: https://github.com/aws/aws-sdk-go#configuring-credentials
