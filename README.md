# TERRAFORM-ASSOCIATE-CERTIFICATION-003

<U>OVERVIEW</U>
1. Explanation of Interpolation sequence
  -code explantion
   resource "aws_s3_bucket" "my_bucket" {
  bucket = "${var.project_name}-bucket-${random_string.suffix.result}"
  acl    = "private"

  tags = {
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
}

**Resource Type and Name.**
*resource "aws_s3_bucket" "my_bucket"
  -Creates an S3 bucket.
  -The local name my_bucket is how other resources can reference it in the Terraform project.

**Bucket**
bucket = "${var.project_name}-bucket-${random_string.suffix.result}"
Dynamically generates the bucket name using:

var.project_name → a variable that stores the project's name.

random_string.suffix.result → a random string suffix for uniqueness.

**Access Control**
acl = "private"
Sets the bucket to private, meaning no public access by default.

**Tags**
tags = {
  Environment = var.environment
  ManagedBy   = "Terraform"
}
Adds metadata to the bucket:

Environment → value comes from the var.environment variable.

ManagedBy → indicates Terraform manages the resource.

2. Exploring data sources
   data "aws_ami" "example" {
  most_recent = true

  owners = ["self"]
  tags = {
    Name   = "app-server"
    Tested = "true"
  }
}

A data block requests that Terraform read from a given data source ("aws_ami") and export the result under the given local name ("example"). The name is used to refer to this resource from elsewhere in the same Terraform module, but has no significance outside of the scope of a module.

**Terraformmodule**
A Terraform module is a set of Terraform configuration files in a single directory. Even a simple configuration consisting of a single directory with one or more .tf files is a module. When you run Terraform commands directly from such a directory, it is considered the root module. So in this sense, every Terraform configuration is part of a module. You may have a simple set of Terraform configuration files such as:

.
├── LICENSE
├── README.md
├── main.tf
├── variables.tf
├── outputs.tf
