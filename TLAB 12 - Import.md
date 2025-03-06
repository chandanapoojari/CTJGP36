## Understanding Terraform Import
```
mkdir import_lab && cd import_lab
```
**Create an ubuntu EC2 instance on AWS via the management console with default configurations**
```
vi import.tf
```
Add the given lines, by pressing "INSERT" 
```
provider "aws" {
  region = "us-east-2"
}   

resource "aws_instance" "test_instance" {
  ami = ""
  instance_type = ""
  tags = {
    Name = ""
  }
}
```
Save the file using "ESCAPE + :wq!"

```
terraform init
```
```
terraform import aws_instance.test_instance instanceid
```
```
cat terraform.tfstate
```

**Note** : Copy the AMI ID, Instance type and Name of the resource from the state file and add it to import.tf
```
terraform plan
```
```
terraform apply
```
```
terraform destroy
```
```
cd ..
```
```
rm -rf import_lab
```
