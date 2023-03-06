---
tags:
  - IT
  - Cloud
  - IaaC
  - Terraform
---
 - [cert](https://www.hashicorp.com/certification/terraform-associate)
# General
- [[Terraform]] assumes [[Immutable infrastructure]] approach
 
# modules
- Module is a container for collention of resources meant to be used together
- Module includes set of `.tf` or (`.tf.json`) files from one directory
- Root module - Main module which is referencing other modules
- Child modules - module called by another module
- Modules can be published to Terraform registry
- modules can be reused (called multiple times with different input variables) within single Root Module
- moving existing resources between the modules in the codebase would lead to recreating them, you can use [refactoring blocks](https://developer.hashicorp.com/terraform/language/modules/develop/refactoring) to address it

## Calling a Child Module
```hcl
module "servers" {           # "server" is a local name
  source = "./some-module    # local module path
  servers = 5                # custom input variable
}
```
- `terraform init` - adjusting installed modules
	- `-upgrade` allows for updating already installed modules
 
## Modules compostion 
- Modules three should be rather flat - modules should be passed with their dependencies when being call from root module instead of referencing them directly as their child modules

## Data-only modules
- There can be modules which are not provisioning any resources but just qerying the Cloud API and providing information about resources to other modules
```hcl
data "aws_ami" "example" {
  most_recent = true

  owners = ["self"]
  tags = {
    Name   = "app-server"
    Tested = "true"
  }
}
```

## No-Code ready modules
- Standarize collections of infrastructure resources that downstream userse can easily deploy
- Should follow [standard module structure](https://developer.hashicorp.com/terraform/language/modules/develop/structure)
- Variable defaults should be dfined if possible
- Should be built for specific use case

## versioning
```hcl
module "servers" {            
  source = "hashicorp/consul/aws"   
  # version only supported for modules stored in module registry
  version = "0.0.5"          
}
```
- Only modules stored in the registry can be versioned (local modules are not versioned)
- Modules from other locations can versioned with `source` identifier

## output variables
```hcl
output "instance_ip_addr" {
  value = aws_instance.server.private_ip
}
```
- Child module can declare output variables to export some metadata to other modules
- Exported outputs can be referenced as input variables for other modules:
```hcl
resource "aws_elb" "example" {
  # ...

  instances = module.servers.instance_ids
}
```
- output variables defined by Root Module are printed by CLI after execution

### preconditions
- you can set `preconditions` which will be evaluated before exportin the output
```hcl
output "api_base_url" {
  value = "https://${aws_instance.example.private_dns}:8433/"

  # The EC2 instance must have an encrypted root volume.
  precondition {
    condition     = data.aws_ebs_volume.example.encrypted
    error_message = "The server's root volume is not encrypted."
  }
}
```
- `description` can be added
- `sensitive` keywoard can be use to obfuscate data from the output (value is still saved in terraform state)

## syntax
### depends_on
```hcl
resource "aws_iam_role" "example" {
  name = "example"

  # assume_role_policy is omitted for brevity in this example. Refer to the
  # documentation for aws_iam_role for a complete example.
  assume_role_policy = "..."
}

resource "aws_iam_instance_profile" "example" {
  # Because this expression refers to the role, Terraform can infer
  # automatically that the role must be created first.
  role = aws_iam_role.example.name
}

resource "aws_iam_role_policy" "example" {
  name   = "example"
  role   = aws_iam_role.example.name
  policy = jsonencode({
    "Statement" = [{
      # This policy allows software running on the EC2 instance to
      # access the S3 API.
      "Action" = "s3:*",
      "Effect" = "Allow",
    }],
  })
}

resource "aws_instance" "example" {
  ami           = "ami-a1b2c3d4"
  instance_type = "t2.micro"

  # Terraform can infer from this that the instance profile must
  # be created before the EC2 instance.
  iam_instance_profile = aws_iam_instance_profile.example

  # However, if software running in this EC2 instance needs access
  # to the S3 API in order to boot properly, there is also a "hidden"
  # dependency on the aws_iam_role_policy that Terraform cannot
  # automatically infer, so it must be declared explicitly:
  depends_on = [
    aws_iam_role_policy.example
  ]
}
```

- this pattern shouldn't be abused beacuse it limits how Terraform can construct the plan

### count
`count` can be used to create multiple resources with similar configuration
```hcl
resource "aws_instance" "server" {
  count = 4 # create four similar EC2 instances

  ami           = "ami-a1b2c3d4"
  instance_type = "t2.micro"

  tags = {
    Name = "Server ${count.index}"
  }
}
```

You can refer to created resource by its index eg. `aws_instance.server[0]`

### for each
```hcl
resource "azurerm_resource_group" "rg" {
  for_each = {
    a_group = "eastus"
    another_group = "westus2"
  }
  name     = each.key
  location = each.value
}
```
- some collection can be passed to `for_each`
- Terraform will create separate resources fo every item
- `each.key` and `each.value` can be referenced
- you can chain resources with for each 

### for expressions
```hcl
[for o in var.list : o.id]
```

### splat expressions
```hcl
var.list[*].id
```

### conditionals
```hcl
var.a != "" ? var.a : "default-a"
```

### data structures
- list 
- map 
- set

### string templates
```hcl
"Hello, ${var.name}!"
```

### directives
```hcl
"Hello, %{ if var.name != "" }${var.name}%{ else }unnamed%{ endif }!"
```

### Whitespace Stripping
```hcl
<<EOT
%{ for ip in aws_instance.example.*.private_ip ~}
server ${ip}
%{ endfor ~}
EOT
```
# Provisioners
- shouldn't be used if not necessary
```hcl
resource "aws_instance" "web" {
  # ...

  provisioner "local-exec" {
    when    = destroy
    command = "echo 'Destroy-time provisioner'"
    on_failure = continue
  }
}
```
- You can run some command on the local machine as part of  resource creation
- `when` allows to run provisioner on the specific event 
- With `on_failure` you can decide what impact  failure of provisioner execution should have 
- Most popular provisioners
	- `local-exec`
	- `remote-exec`
# Backends
It can be used to define a backend for the terrafrom state (eg. S3) and DynamoDB table for locking
```hcl
# stage/frontend-app/main.tf
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "stage/frontend-app/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "my-lock-table"
  }
}
```

# Providers
## AWS

# state
- Terrafrom tracks state and metadata about cloud resources
- Terraform tracks relations between resources in state and configuration files
- by default state is kept it `terraform.tfstate` in locar directory ([[JSON]] file)
- state can be also kept remotely
- Terrafrom always runs `refresh` command prior to the update, to fetch actual state
	- `--refresh=false` can be set to skip the refresh
- `terraform state rm` - removes resource from the state 

## remote state
- state can be kept remotely on
	- Terraform Cloud
	- [[Consul]]
	- S3
- remote state can be configured in Root Module

## import
- `terraform import` - allows to import existing resource to the state

## Format 
- Format of state file may change between terraform release
- `terrafrom output -json` allows to export outputs in standarized json for external procssing
- `terraform show -json` - allows to export state snapshot as [[JSON]] file which can be compared with previous plans 
- It makes sense to run those `show` and `output` commands staright after running `terraform apply` 

# Workspace

# CDK
...

# Terraform Cloud
- RBAC
- Shared state
- Shared secrets
- Private registry for modules

# Terragrunt
- Terraform wrapper which adds some features to keep configuration DRY ( Do not repeat yourself )
- allows to run various hooks
- allows to pass some parameters from CLI 
- allows to inject common configuration into multiple modules (eg. backend configuration)
...

# Good practices
- How to structure Terraform code 
- 