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
