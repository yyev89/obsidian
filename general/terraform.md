simple main.tf example file for creation aws ec2 instance:
```hcl
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
```

destroy managed infra:
```bash
terraform destroy
```

