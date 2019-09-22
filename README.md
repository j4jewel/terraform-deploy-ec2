####  Provisioning an ec2 instance along with security group using Terraform

Terraform is a tool made by Hashicorp for building, changing, and versioning infrastructure safely and efficiently. Terraform can manage existing and popular service providers ( aws, azure, Google cloud) as well as custom in-house solutions.

### Prerequisite:
 Download Terraform from [link](https://www.terraform.io/downloads.html) and set up it in a linux machine. ( Here I'm using a ec2 instance)  Configure aws cli in the instance. 
 
 ##### mkdir ec2
 ##### cd ec2/
 ##### vim ec2.tf
 #
 ```sh
 provider "aws" {

    region = "us-east-1"

}

resource "aws_instance" "instance1" {

   ami = "ami-0b898040803850657"
   instance_type = "t2.micro"
   key_name = "jwl-pc"
   availability_zone = "us-east-1b"
   associate_public_ip_address = true
   vpc_security_group_ids = ["${aws_security_group.sg.id}"]
   tags = {
     Name = "TF-Server"
   }
}

resource "aws_security_group" "sg" {

   name = "tfsecurity"
   
   tags = {
       Name = "TF-Webserver"
   }

   ingress {
      from_port = 22
      to_port = 22
      protocol = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
   }

   ingress {
      from_port = 80
      to_port = 80
      protocol = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
   }

   ingress {
      from_port = 443
      to_port = 443
      protocol = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
   }

   egress {
     from_port = 0
     to_port = 0
     protocol = "-1"
     cidr_blocks = ["0.0.0.0/0"]
   }
    
}
```
##### Execution
#
```sh
#terraform validate   - syntax check 

#terraform plan - Creating an execution plan ( to check what will get installed before running it)

# terraform apply - Applying

# terraform destroy - Destroying what we have applied through terrafrom apply

We can also use -auto-approve while applying.
# terraform apply -auto-approve

```
