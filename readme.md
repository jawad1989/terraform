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
* alertnate approach
terraform plan -var=instancetype=t2.small

* creating a *terraform.tfvars* file


### Notes:
https://docs.google.com/document/d/179clqsxOGQa-iGKu1dcmz89Vpso9-7Of8opIkXwPr_k/edit
