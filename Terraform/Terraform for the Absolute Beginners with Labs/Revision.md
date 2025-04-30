# Section 4: Terraform Basics

## Terraform for the Absolute Beginners with Labs@Udemy

https://www.udemy.com/course/terraform-for-the-absolute-beginners/learn/lecture/27884516#overview
https://www.udemy.com/course/terraform-beginner-to-advanced/learn/lecture/37097118#overview


## 3 Types of Providers

- Terraform Providers
- Partner Providers
- Community Providers

Partner Providers are not maintained by HashiCorp.
Partner and Community Providers require `required_providers` block.
`terraform.required_providers` block defines source and version of providers.
`provider` block defines the parameter of a provider like `region` and credentials.

```providers.tfm
terraform {
  required_providers {
    aws = {
      source = "digitalocean/digitalocean"
      version = "~>2.0"
    }
  }
}

provider "digitalocean" {
  token = "TOKEN"
}
```

## Resource Definition

### Configuration File Example

```
resource "local_file" "key_details" {
  filename = "/root/test.txt"
  content = "This is a test."
}
```

## Variables Definition Precedence

1. Environment variables
2. terraform.tfvars
3. terraform.tfvars.json
4. \*.auto.tfvars or \*.auto.tfvars.json
5. -var or -var-file command line flags

### tf Example

```
variable "filename" { default = "/root/test.txt"}
variable "content" { default = "This is a test." }
```

### tfvars Example

```
filename = /root/test.txt
content = "This is a test."
```

## Variable Referencing in String - ${}

```variable
session_token = "${var.service_name}:${var.service_token}"
```

## Not Allowed Variable Names

- count
- depends_on
- for_each
- lifecycle
- providers
- source

## Resource Referencing

```
resource "tls_private_key" "pvtkey" {
  algorithm = "RSA"
  rsa_bits = 4096
}

resource "local_file" "key_details" {
  filename = "/root/key.txt"
  content = tls_private_key.pvtkey.private_key_pem
}
```

### Resource Referencing - wildcard

```
resource "aws_instance" "example" {
  ami = "ami-abc123"
  instance_type = "t2.micro"
}

ebs_block_device {
  device_name = "sda2"
  volume_size = 16
}

ebs_block_device {
  device_name = "sda3"
  volume_size = 20
}
```

```
aws_instance.example.ebs_block_device.*.volume_id
```

### Resource Referencing - depends_on

```
resource "local_file" "whale" {
  filename = "/root/whale"
  content = "whale"
  depends_on = [ local_file.krill ]
}

resource "local_file" "krill" {
  filename = "/root/krill"
  content = "krill"
}
```

Both /root/whale and /root/krill will be created.

## Output Variables

```
output "instance_ip_addr" {
  value = aws_instance.server.private_ip
}

```

# Section 5: Terraform State

## Terraform Workflow

1. Write

- Write a configuration file in main.tf
- Write variables in variables.tf
- Write outputs in outputs.tf

2. Initialize (install provider plugins)

```
terraform init
```

1. Plan (create/update terraform.tfstate)

```
terraform plan -out=filename
terraform plan -var-file=filename
terraform plan -refresh-only (does not create/update resources and the state file)
```

3. Apply (create/update terraform.tfstate)

```
terraform apply
terraform apply -auto-approve
terraform apply -refresh-only (does not create/update resources and the state file)
```

## Terraform State

Store the dependencies information for resource deletion.

## Remote State

# Section 6: Working with Terraform

4. Validate

```
terraform validate
terraform fmt
terraform show
terraform graph
terraform output
terraform plan -out=filename
terraform providers
terraform provider mirror /home/provider.tf
terraform apply -refresh-only # Update the state file only
```

5. Destroy

```
terraform destroy
```

## Lifecycle Rules

- create_before_destroy
- prevent_destroy
- ignore_changes
- replace_triggered_by
- create_replace
- update_replace
- delete_before_replace

### Lifecycle rules Example

```
resource "aws_instance" "Example" {
  lifecycle {
    create_before_destroy = true
  }
}
```

## Data Sources

Read-only resources.

```
data "aws_ami" "ubuntu" {
  most_recent = true
  owners = ["amazon"]
}
```

## Sensitive Parameter

If Output sensitive data, error will be shown. Unless `sensitive = true` parameter is set, the Output would be `(sensitive value)`.
Sensitive parameter `sensitive = true` does NOT protect or redact information in State file.
Mature vendors like Amazon set sensitive parameter by default for their resources.

### Sensitive Variable

```
variable "password" {
  default = "password"
  sensitive = true
}
```

### Sensitive Resource

```
resource "local_sensitive_file" "name" {
  filename = "/root/password"
  content = "password"
}
```

# Meta Arguments

## count

Output is a list.

```main.tf
resource "local_sensitive_file" "name" {
  filename = var.users[count.index]
  content = var.content
  count = length(var.users)
}
```

```variables.tf
variable "users" {
    type = list(string)
    default = [ "/root/user10", "/root/user11", "/root/user12", "/root/user10"]
}
variable "content" {
    default = "password: S3cr3tP@ssw0rd"

}
```

## for_each

Only accepts a map or a set.
Output is a map.

```main.tf
resource "local_file" "pet" {
  filename = each.value
  for_each = var.filename
}
```

```variables.tf
variable "filename" {
  type = set(string) # or map(string)
  default = [
    "/root/pets.txt",
    "/root/dogs.txt",
    "/root/cats.txt"
  ]
}
```

Resource can be a list or a map depends on how you create.

## depends_on

## lifecycle

## provider

## Version Constraints

```main.tf
resource "local_sensitive_file" "name" {
    filename = each.value
    content = var.content
    for_each = var.users
}
```

```variables.tf
variable "users" {
    type = set(string)
    default = [ "/root/user10", "/root/user11", "/root/user12", "/root/user10"]
}
variable "content" {
    default = "password: S3cr3tP@ssw0rd"
}
```

## Dynamic Block

```
resource "aws_elastic_beanstalk_environment" "tfenvtest" {
  name                = "tf-test-name"
  application         = aws_elastic_beanstalk_application.tftest.name
  solution_stack_name = "64bit Amazon Linux 2018.03 v2.11.4 running Go 1.12.6"

  dynamic "setting" {
    for_each = var.settings
    content {
      namespace = setting.value["namespace"]
      name = setting.value["name"]
      value = setting.value["value"]
    }
  }
}
```

# Section 7: Terraform with AWS

## AWS Provider

### AWS Provider with Custom Endpoint

```
provider "aws" {
  region                      = "us-east-1"
  skip_credentials_validation = true
  skip_requesting_account_id  = true

  endpoints {
    iam                       = "http://aws:4566"
  }
}
```

## AWS Resources

### AWS Instance

```
resource "aws_instance" "Example" {
  ami = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

### AWS IAM User

```
resource "aws_iam_user" "users" {
  name = "mary"
}
```

### AWS DynamoDB Table

```
resource "aws_dynamodb_table" "project_sapphire_user_data" {
  name           = "userdata"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "UserId" # Primary key

  attribute {
    name = "UserId"
    type = "S"
  }
}
```

### AWS DynamoDB Table Item

```
resource "aws_dynamodb_table" "project_sapphire_inventory" {
  name         = "inventory"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "AssetID"

  attribute {
    name = "AssetID"
    type = "N"
  }
  attribute {
    name = "AssetName"
    type = "S"
  }
  attribute {
    name = "age"
    type = "N"
  }
  attribute {
    name = "Hardware"
    type = "B"
  }
  global_secondary_index {
    name            = "AssetName"
    hash_key        = "AssetName"
    projection_type = "ALL"

  }
  global_secondary_index {
    name            = "age"
    hash_key        = "age"
    projection_type = "ALL"

  }
  global_secondary_index {
    name            = "Hardware"
    hash_key        = "Hardware"
    projection_type = "ALL"

  }
}
resource "aws_dynamodb_table_item" "upload" {
  table_name = aws_dynamodb_table.project_sapphire_inventory.name
  hash_key   = aws_dynamodb_table.project_sapphire_inventory.hash_key
  item        = <<KEY
  {
    "AssetID": {"N": "1"},
    "AssetName": {"S": "printer"},
    "age": {"N": "5"},
    "Hardware": {"B": "true" }
  }
  KEY
}
```

### AWS S3 Bucket

```
resource "aws_s3_bucket" "project_sapphire" {
  bucket = "project-sapphire"
  acl    = "private"

  tags = {
    Name = "project-sapphire"
  }
}
```

## Vault Provider

### Vault Integration Example

```
provider "vault" {
  address = "http://127.0.0.1:8200"
}

data "vault_generic_secret" "demo" {
  path = "secret/db-creds"
}

output "vault_secrets" {
  value = data.vault_generic_secret.demo.data_json
  sensitive = "true"
}
```

# Section 8: Remote State

## Purpose of State file

- Mapping configuration to real-world resources
- Tracking metadata
- Performance optimization
- Collaboration

## Purpose of Remote State

- Security
- Centralized state management
- Collaboration
- State locking (which Git systems cannot provide)

## Remote State Providers

- Terraform Cloud
- HashiCorp Consul
- Amazon S3 (state file) ＋ DynamoDB (state lock)
- Azure Blob Storage
- Google Cloud Storage

### S3 Bucket as Remote State Backend with DynamoDB Locking

```
terraform {
  backend "s3" {
    key = "terraform.tfstate"
    region = "us-east-1"
    bucket = "remote-state"
    endpoint = "http://172.16.238.105:9000"
    force_path_style = true


    skip_credentials_validation = true

    skip_metadata_api_check = true
    skip_region_validation = true
  }
}
```

## Terraform State Commands

```
terraform state list
terraform state show
terraform state mv
terraform state pull (test if remote state is working)
terraform state rm (Remove the resource from Terraform management, not an actual resource deletion)
terraform state replace-provider
terraform state push
terraform state push -force
```

# Section 9: Terraform Provisioners

### ⚠️ AWS Instance

```
resource "aws_instance" "webserver" {
  ami = "ami-0edab43b6fa892279"
  instance_type = "t2.micro"

  tags = {
    Name = "webserver"
    Description = "Nginx Web Server"
  }
  user-data = <<-EOF
    #!/bin/bash
    sudo yum update -y
    sudo yum install nginx -y
    sudo systemctl start nginx
    sudo systemctl enable nginx
  EOF

  key_name = aws_key_pair.web.id # SSH key

  vpc_security_group_ids = [ aws_security_group.ssh-access.id ] # Security group
}

resource "aws_key_pair" "web" {
  key_name = "web"
  public_key = file("/root/.ssh/web.pub")
}

resource "aws_security_group" "ssh-access" {
  name = "ssh-access"
  description = "Allow SSH access from the Internet"
  ingress = {
    from_port = 22
    to_port = 22
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

output publicip {
  value = aws_instance.webserver.publicip
}
```

## Terraform Provisioners

- No Provisioner information is shown in Plan
- Network connectivity and Authentication have to be in place before execution
- A best fit or custom built image should be used
- `connection` block is used for authentication

### SSH Provisioner Example

```
# Copies the file as the root user using SSH
provisioner "file" {
  source      = "conf/myapp.conf"
  destination = "/etc/myapp.conf"

  connection {  # For authentication
    type     = "ssh"
    user     = "root"
    password = var.root_password
    host     = var.host
  }
}

# Copies the file as the Administrator user using WinRM
provisioner "file" {
  source      = "conf/myapp.conf"
  destination = "C:/App/myapp.conf"

  connection {
    type     = "winrm"
    user     = "Administrator"
    password = var.admin_password
    host     = var.host
  }
}
```

### Remote Provisioner (Run commands on remote host)

```
resource "aws_instance" "webserver" {
  ami = "ami-0edab43b6fa892279"
  instance_type = "t2.micro"

  tags = {
    Name = "webserver"
    Description = "Nginx Web Server"
  }

  provisioner "remote-exec" = {
    inline = [
      "sudo apt update",
      "sudo apt install nginx -y",
      "sudo systemctl enable nginx",
      "sudo systemctl start nginx",
    ]
  }

  connection {
    type = "ssh"
    host = self.public_ip
    user = "ubuntu"
    private_key = file("/root/.ssh/web")
  }

  key_name = aws_key_pair.web.id # SSH key

  vpc_security_group_ids = [ aws_security_group.ssh-access.id ] # Security group
}


resource "aws_key_pair" "web" {
  ...
}

resource "aws_security_group" "ssh-access" {
  ...
}
```

### Local Provisioner (Run commands on local host)

#### On Time and Failure Provisioners

```
resource "aws_instance" "webserver" {
  ami = "ami-0edab43b6fa892279"
  instance_type = "t2.micro"

  # Default: Create time provisioner
  provisioner "local-exec" {
    command = "echo ${aws_instance.webserver.public_ip} >> /tmp/ips.txt"
  }

  # Destroy time provisioner
  provisioner "local-exec" {
    when = destroy
    command = "echo ${aws_instance.webserver.public_ip} >> /tmp/ips.txt"
  }

  # On failure provisioner
  provisioner "local-exec" {
    on_failure = fail # Or continue
    command = "echo ${aws_instance.webserver.public_ip} >> /tmp/ips.txt"
  }
```

# Section 10: Terraform Import, Tainting Resources and Debugging

## Taint

- Tainting a resource forces Terraform to destroy and recreate the resource on the next plan/apply
- Tainting a resource is useful for testing
- Tainting a resource is also useful for debugging

```
terraform taint aws_instance.webserver
terraform untaint aws_instance.webserver
```

## Debugging

- Terraform has a built-in debugger
- The debugger is not available for all resources
- The debugger is not available for all providers
- The debugger is not available for all versions of Terraform

```
export TF_LOG=TRACE
export TF_LOG_PATH=/tmp/tf.log # TF_LOG has to be set

terraform apply -auto-approve -input=false -debug
```

## Terraform Import

- `terraform import` is used to import existing resources into Terraform state
- `terraform import` is used to import resources that are not managed by Terraform
- `terraform import` is used to import resources that are managed by another tool
- `terraform import` does not update Configuration file at all but the State file
- `terraform import` can generate a configuration file with `terraform plan` command

```
resource "aws_instance" "webserver-2" {}
```

```
terraform import aws_instance.webserver i-01234 # resource block has to be in placed before this
terraform plan # Refresh and review the update
```

## Terraform Import Example

```
import {
  to = aws_security_group.my_sg
  id = "sg-123456"
}
```

```
terraform plan -generate-config-out=plan.tf
terraform apply
```

# Section 11: Terraform Modules

- Configuration separation within a Project

## Drawback of Using Single Large or Multiple Small Configuration Files

- Complex configuration files
- Duplicated codes
- Increased risk and human errors
- Limited reusability

## Local Modules Example

```
module "webserver" {
  source = "./modules/webserver"
  name = "webserver"
  subnet_id = "subnet-123456"
  vpc_id = "vpc-123456"
}
```

## Remote Modules Example

```
module "security-group_ssh" {
  source = "terraform-aws-modules/security-group/aws/modules/ssh"
  version = "3.16.0"
  name = "ssh-access"
  vpc_id = "vpc-7d8d215"
  ingress_cidr_blocks = [ "10.10.0.0/16" ]
}
```

## Download Remote Modules

```
terraform get
```

# Section 12: Terraform Functions and Conditional Expressions

```
terraform console
```

```
file("/root/terraform-projects/main.tf")
length(var.region)
toset(var.region)
```

## Numeric Functions

- min(), max()
- count()
- length()
- celi()
- floor()

## String Functions

- lower()
- upper()
- title()
- split("delimiter", string)
- join("delimiter", string)
- substr(string, start_index, length) -> substring
- length()
- index(string, "keyword") -> index
- element(string, index) -> string
- contains(string, "keyword") -> boolean

## Collection Functions

### ⚠️ Map Functions

- keys(map) -> list
- values(map) -> list
- lookup(map, "key") -> value
- lookup(map, "key", "default_value") -> value

## Type Conversion Functions

## Conditional Expressions

### if-then-else Example

condition ? true_value : false_value

```
length = var.length < 8 ? 8 : var.length
```

---

### Example

```main.tf
resource "aws_iam_user" "cloud" {
     name = split(":",var.cloud_users)[count.index]
     count = length(split(":",var.cloud_users))

}
resource "aws_s3_bucket" "sonic_media" {
     bucket = var.bucket

}

resource "aws_s3_object" "upload_sonic_media" {
  for_each = var.media
  source = each.value
  bucket = aws_s3_bucket.sonic_media.id
  key = split("/", each.value)[length(split("/", each.value))-1]
}
```

```variables.tf
variable "region" {
  default = "ca-central-1"
}
variable "cloud_users" {
     type = string
     default = "andrew:ken:faraz:mutsumi:peter:steve:braja"

}
variable "bucket" {
  default = "sonic-media"

}

variable "media" {
  type = set(string)
  default = [
    "/media/tails.jpg",
    "/media/eggman.jpg",
    "/media/ultrasonic.jpg",
    "/media/knuckles.jpg",
    "/media/shadow.jpg",
      ]

}
variable "sf" {
  type = list
  default = [
    "ryu",
    "ken",
    "akuma",
    "seth",
    "zangief",
    "poison",
    "gen",
    "oni",
    "thawk",
    "fang",
    "rashid",
    "birdie",
    "sagat",
    "bison",
    "cammy",
    "chun-li",
    "balrog",
    "cody",
    "rolento",
    "ibuki"

  ]
}
```

## Terraform Workspaces (OSS)

- Allow different projects share the same state file.
- Project based separation.

```
terraform workspace list
terraform workspace new <workspace_name>
terraform workspace select <workspace_name>
terraform workspace delete <workspace_name>
```

### Terraform workspace Referencing

```main.tf
module "payroll_app" {
  source = "/root/terraform-projects/modules/payroll-app"
  app_region = var.region[terraform.workspace]
  ami = var.ami[terraform.workspace]

#   app_region = lookup(var.region, terraform.workspace)
#   ami        = lookup(var.ami, terraform.workspace)
}
```

```variables.tf
variable "region" {
    type = map
    default = {
        "us-payroll" = "us-east-1"
        "uk-payroll" = "eu-west-2"
        "india-payroll" = "ap-south-1"
    }

}
variable "ami" {
    type = map
    default = {
        "us-payroll" = "ami-24e140119877avm"
        "uk-payroll" = "ami-35e140119877avm"
        "india-payroll" = "ami-55140119877avm"
    }
}
```

## Terraform State (OSS)

```
terraform state list
terraform state show <resource_name>
```

# Mock Exam

- Terraform Security
  - Enhanced remote backend
  - Define secrets in a connection configuration outside of Terraform
- Terraform Providers
  - Terraform providers does not take responsible for multi cloud issues
  - `terraform` block
  - `connection` block. Refer to [SSH Provisioner Example](#ssh-provisioner-example)
  - Provide security as Terraform searches for login credentials (eg. AWS) automatically
  - `required_providers` vs `provider` block [3 Types of Providers](#3-types-of-providers)
- `terraform import` vs `terraform workspace` usages
  - `import`: Import existing infrastructure resources into Terraform
  - `workspace`: Share the same configuration and state file across multiple environments
- `dynamic` block is supported inside ``resource`, `data`, `provider`, and `provisioner` blocks
  - A `dynamic` block acts much like a for expression, but produces nested blocks instead of a complex typed value. It iterates over a given complex value, and generates a nested block for each element of that complex value
- Variable inheritance
  - `node_count` input variable
- Terraform Enterprise
  - Sentinel policy
    - Able to define preconditions for Terraform runs
    - Policy as Code
      - Policy as code is a way to define policies in a file that can be checked into version control
