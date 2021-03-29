Table On Content:

# Section2:
## variables
## variable association
## count
## data types
## Contional Expressions
## Local values
## functions


### Resource
### Provider
### Commands:
```
terraform init
terraform plan
terraform apply
terraform destroy
terraform refresh   # fetches  current state and updates state file
```
### Terraform state
> Terraform tries to ensure that deployed infra is based on desired state( what we added in tf file) rather current state

### Provider versions
```
>=1.0           greater then equal to version
~>2.0           any version in 2.x range
>=2.0,<=2.30    between
```

### Dependency Lock File
allows us to lock a specific version of provider

### association
how we can associate two resources e.g. Elastic Ip with EC2 instance
```
resource "aws_eip_association" "eip_assoc" {
  instance_id   = aws_instance.jsec2.id
  allocation_id = aws_eip.lb.id
}

```

### variables:

* variables.tf
```
variable "instancetype" {
    default = "t2.micro"
}
```

* main.tf
```
resource "aws_instance" "jsec2" {
    ami           = "ami-0533f2ba8a1995cf9"
    instance_type =var.instancetype
}

```

### variable assignment

* alertnate approach

terraform plan -var=instancetype=t2.small

* creating a *terraform.tfvars* file  ( BEST APPROACH )

  this will overwrite value in default var file

* terraform plan -var-file="custom.tfvars"

  if you are not using terraform.tfvars file
  
 * setting env var for variables
  
   export TF_VAR_instancetype="m5.large"
   
   unset TF_VAR_instancetype
  
* Data Types:

Type: String, list, map, number, bool
```
variable "instance_name" {
  type = number 
} 
```

* fetch data in var from map/list
```

resource "aws_instance" "jsec2" {
    ami           = "XXXXXXXXXXXXXXXXX"
     instance_type = var.types["us-west-2"]
}

---var file
variable "types" {
  type    = map
  default = {
      us-east-1  = "t2.micro"
      us-west-2  = "t2.nano"
      us-south-1 = "t2.small"
  }
}


```


* count and count index

```
provider "aws" {
    region = "us-east-1"
    access_key = "AKIA5GHD6OLXPMPBJIF5"
    secret_key = "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" 
}

variable "elb_names" {
  type    = list
  default = ["prod","dev","UAT"]
}

variable "types" {
  type    = map
  default = {
      us-east-1  = "t2.micro"
      us-west-2  = "t2.nano"
      us-south-1 = "t2.small"
  }
}

resource "aws_instance" "js-instance1" {
    ami           = "ami-0533f2ba8a1995cf9"
    instance_type = var.types["us-east-1"]
    count         = 3
    tags = {
      Name = var.elb_names[count.index]
    }
}

output "jawad" {
  value = aws_instance.js-instance1[*].public_ip
}


```

#### Create three IAM users

```
variable "elb_names" {
  type    = list
  default = ["prod","dev","UAT"]
}

resource "aws_iam_user" "iam" {
  name = var.elb_names[count.index]
  count = 3
  path = "/system/"
}
```


#### Conditional Expression 

> condition ? true-val : false-val
```
variable "is_dev" {}

resource "aws_instance" "dev" {
    ami           = "ami-0533f2ba8a1995cf9"
    instance_type = "t2.micro"
    count =  var.is_dev == true ? 3 : 0
    tags = {
    Name = "DEV"
  }
  
}

resource "aws_instance" "prod" {
    ami           = "ami-0533f2ba8a1995cf9"
    instance_type = "t2.micro"
    count =  var.is_dev == true ? 0 : 1
    tags = {
    Name = "PROD"
  }
  
}
```


#### Local Values

```
locals {
  common_tags = {
    Owner = "DevOps Team"
    service = "backend"
  }
}
```

### functions:

https://www.terraform.io/docs/language/functions/index.html

```
terraform console
max(1,23,4)
> 23
```
### Notes: 
https://docs.google.com/document/d/179clqsxOGQa-iGKu1dcmz89Vpso9-7Of8opIkXwPr_k/edit
