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

3. **modulevpc** 

explanation
provider "aws" {
  region = "us-west-2"

  default_tags {
    tags = {
      hashicorp-learn = "module-use"
    }
  }
}

module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "3.18.1"

  name = var.vpc_name
  cidr = var.vpc_cidr

  azs             = var.vpc_azs
  private_subnets = var.vpc_private_subnets
  public_subnets  = var.vpc_public_subnets

  enable_nat_gateway = var.vpc_enable_nat_gateway

  tags = var.vpc_tags
}

module "ec2_instances" {
  source  = "terraform-aws-modules/ec2-instance/aws"
  version = "4.3.0"

  count = 2
  name  = "my-ec2-cluster-${count.index}"

  ami                    = "ami-0c5204531f799e0c6"
  instance_type          = "t2.micro"
  vpc_security_group_ids = [module.vpc.default_security_group_id]
  subnet_id              = module.vpc.public_subnets[0]

  tags = {
    Terraform   = "true"
    Environment = "dev"
  }
}

Explanation of the above resource module.
This configuration includes three blocks:

The provider "aws" block configures the AWS provider. Depending on the authentication method you use, you may need to include additional arguments in the provider block.
The module "vpc" block configures a Virtual Private Cloud (VPC) module, which provisions networking resources such as a VPC, subnets, and internet and NAT gateways based on the arguments provided.
The module "ec2_instances" block defines two EC2 instances provisioned within the VPC created by the module.

4. Terraform Lifecycle rules

   Lifecycle arguments help control the flow of your Terraform operations by creating custom rules for resource creation and destruction. Instead of Terraform managing operations in the built-in dependency graph, lifecycle arguments help minimize potential downtime based on your resource needs as well as protect specific resources from changing or impacting infrastructure.

5. Terraform Dependency graph
   Terraform's dependency graph is a fundamental concept used to manage the order of resource creation, updates, and deletion. It represents the relationships between resources and modules within a Terraform configuration.

6 HCP Terraform provides four imports to define policy rules for the plan, configuration, state, and run associated with a policy check.

tfplan - Access a Terraform plan, which is the file created as a result of the terraform plan command. The plan represents the changes that Terraform must make to reach the desired infrastructure state described in the configuration.
tfconfig - Access a Terraform configuration. The configuration is the set of .tf files that describe the desired infrastructure state.
tfstate - Access the Terraform state. Terraform uses state to map real-world resources to your configuration.
tfrun - Access data associated with a run in HCP Terraform. For example, you could retrieve the run's workspace.

7. default
The default argument defines a default value for the variable, making it optional to provide a value. If you do not provide a variable value when using a module, Terraform uses the default value.

8. What are Terraform Editions?
Cloud: SaaS application that runs Terraform in a stable, remote environment and securely stores state and secrets
Enterprise: allows you to set up private instances of Terraform Cloud  
