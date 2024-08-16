simple main.tf example file for creation aws ec2 instance:
```hcl
terraform {
    required_providers {
        aws = {
            source  = "hashicorp/aws"
            version = "~> 3.0"
        }
    }
}

provider "aws" {
    region = "eu-central-1"
}

data "aws_ami" "amazon_linux_2023" {
    most_recent = true
    owners      = ["amazon"]
    
    filter {
        name   = "name"
        values = ["amzn2-ami-hvm-*-x86_64-gp2"]
    }
}

resource "aws_instance" "example" {
    ami           = data.aws_ami.amazon_linux_2023.id
    instance_type = "t2.micro"

    tags = {
        Name = "Example Instance"
    }
}
```

initialize:
```bash
terraform init
```

verify configuration files:
```bash
terraform validate
```

format configuration:
```bash
terraform fmt
```

generate and show an execution plan:
```bash
terraform plan
```

build or change infra:
```bash
terraform apply
# Don't ask for approval:
terraform apply -auto-approve
```

redeploy specific resource without destroying everything:
```bash
terraform apply -replace <name>
```

refresh terraform state, for example to get output variables added before:
```bash
terraform apply -refresh-only
```

destroy managed infra:
```bash
terraform destroy
```

list created resources:
```bash
terraform state list
# Detailed info about certain resource:
terraform state show <name>
# Detailed info about everything:
terraform show
```
### Variables

- input variables: `var.<name>`
- local variables: `local.<name>`
- output variables

order of input variables precedence (from lowest to highest):
- manual entry during plan or apply
- default value in declaration block
- `TF_VAR_<name>` environment variables
- `terraform.tfvars` file
- `*.auto.tfvars` file
- command line `-var` or `-var-file`

apply additional file with variables:
```bash
terraform apply -var-file=another-variable-file.tfvars
```

pass variable during runtime (but better choice to store it in GitHub or AWS secrets and variables):
```bash
terraform apply -var="db_user=admin" -var="db_pass=secret"
```

to set variable invisible during plan and apply:
```hcl
variable "db_pass" {
    description = "password for db"
    type        = string
    sensitive   = true
}
```

module sources examples:
```hcl
# HTTPS
module "example" {
    source = "github.com/hashicorp/example?ref=v1.2.0"
}
# SSH
module "example" {
    source = "git@github.com:hashicorp/example.git"
}
# GENERIC
module "example" {
    source = "git::ssh://username@example.com/storage.git"
}
```

define varibles in `variables.tf` file (where description, type, default values are stored):
```hcl
variable "host_os" {
    type    = string
    default = "linux"
}
```

actual values for variables are stored in `terraform.tfvars` file:
```hcl
host_os = "macos"
```

to access a variable:
```hcl
${var.host_os}
```

check some variables or other stuff in console:
```bash
terraform console
# Get variable:
var.host_os
```

using conditional statement:
```hcl
var.host_os == "windows" ? ["Powershell", "-Command"] : ["bash", "-c"]
```

set output variables in `outputs.tf` to get info:
```hcl
output "dev_ip" {
    value = aws_instance.dev_node.public_ip
}
```

check outputs after provisioning:
```bash
terraform output
```
### Two main approaches:

**Workspaces** - multiple named sections within a single backend

Pros:
- easy to get started
- convinient terraform.workspace expression
- minimizes code duplication

Cons:
- prone to human error
- state stored within same backend
- codebase doesn't unambiguously show deployment configurations

**File structure** - directory layout provides separation, modules provide reuse

Pros:
- isolation of backends
- improved security
- decreased potential for human error
- codebase fully represents deployed state

Cons:
- multiple `terraform apply` required to provision environments
- more code duplication, but can be minimized with modules