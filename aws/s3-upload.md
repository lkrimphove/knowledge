---
title: S3 Upload
draft: false
tags:
  - aws
  - terraform
  - ci-cd
  - iac
---
## S3 Upload

How to upload data to s3 using [[terraform]]
## Upload whole directory to S3 (including sub-directories)

- mime.json has to include all file-endings and their corresponding mime-types, an example containing all mime-types can be found [here](https://github.com/micnic/mime.json)
- using for_each we iterate over each file in the directory and upload it to the bucket

```terraform
locals {  
  mime_types = jsondecode(file("mime.json"))  
}

resource "aws_s3_bucket" "bucket" {  
  bucket = "example-bucket"  
}

resource "aws_s3_object" "object" {  
  for_each = fileset("bucket-content", "**")
  bucket   = aws_s3_bucket.bucket.id  
  key      = "bucket-content/${each.value}"  
  acl      = "private"  
  source   = "bucket-content/${each.value}"  
  content_type = lookup(local.mime_types, split(".", each.value)[1], null)  
  etag     = filemd5("bucket-content/${each.value}")  
}
```

## Upload a config file

- use terraform/terragrunt variables to write them into a config file and upload it to s3

```terraform
resource "aws_s3_object" "config" {  
  bucket  = aws_s3_bucket.bucket.id  
  key     = "conf.json"  
  acl      = "private"
  content_type = "application/json"  
  content = <<EOF  
window.CONFIG =  
  {  
    conf_value_1: ${var.conf_value_1},  
    conf_value_2: ${var.conf_value_2},  
  }  
  EOF  
}
```


---
## Resources
- [list of mime types](https://github.com/micnic/mime.json)