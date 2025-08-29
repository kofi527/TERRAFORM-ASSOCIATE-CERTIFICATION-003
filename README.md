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
