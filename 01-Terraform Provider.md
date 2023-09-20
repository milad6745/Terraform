## Terraform Provider

یک قسمت مهم از اکوسیستم Terraform است که به کاربران امکان می‌دهد با مخازن مختلف ابری یا محلی ارتباط برقرار کنند و منابع مختلفی را در محیط هدف تعریف و مدیریت کنند. هر فراهم‌کننده (Provider) مسئول ترجمه دستورات Terraform به درخواست‌ها و APIهای مخصوص به هر مخزن یا سرویس است.

به عنوان مثال، فراهم‌کننده AWS به شما امکان می‌دهد با سرویس‌های ابری آمازون ارتباط برقرار کنید و منابع مانند EC2 instances، S3 buckets، RDS databases و غیره را مدیریت کنید.

مراحل استفاده از یک فراهم‌کننده به صورت خلاصه به این صورت است:

**تعریف فراهم‌کننده**:
   در کد Terraform، شما باید فراهم‌کننده‌ای که می‌خواهید از آن استفاده کنید را تعریف کنید. برای مثال:

   ```
   provider "aws" {
     region = "us-west-2"
   }
   ```

   در این مثال، فراهم‌کننده AWS تعریف شده است و تنظیمات مربوط به منطقه "us-west-2" ارائه شده است.

**استفاده از منابع فراهم‌کننده**:
   حالا می‌توانید از منابع موجود در فراهم‌کننده استفاده کنید. برای مثال:

   ```
   resource "aws_instance" "example" {
     ami           = "ami-0c94855ba95c71c99"
     instance_type = "t2.micro"
   }
   ```

   در این مثال، یک منبع EC2 instance از فراهم‌کننده AWS استفاده می‌شود.

**اجرای دستورات Terraform**:
   بعد از تعریف منابع، با اجرای دستورات `terraform init`, `terraform plan`, و `terraform apply`، Terraform اقدام به ایجاد منابع در محیط مورد نظر می‌کند.

هر فراهم‌کننده (Provider) برای سرویس یا ابر خاص خود دارای تنظیمات و منابع خاص به خود است که در مستندات آن فراهم‌کننده توضیح داده شده‌اند.

مثالی از فراهم‌کننده AWS:
```
provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "example" {
  ami           = "ami-0c94855ba95c71c99"
  instance_type = "t2.micro"
}
```

در این مثال، از فراهم‌کننده AWS برای ایجاد یک EC2 instance استفاده می‌شود.


نمونه کد برای تعریف کردن provider open stack در terraform

```yml
# Define required providers
terraform {
required_version = ">= 0.14.0"
  required_providers {
    openstack = {
      source  = "terraform-provider-openstack/openstack"
      version = "~> 1.51.1"
    }
  }
}

# Configure the OpenStack Provider
provider "openstack" {
  user_name   = "admin"
  tenant_name = "admin"
  password    = "pwd"
  auth_url    = "http://myauthurl:5000/v2.0"
  region      = "RegionOne"
}

# Create a web server
resource "openstack_compute_instance_v2" "test-server" {
  # ...
}
```
