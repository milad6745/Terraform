برای ایجاد یک ماشین مجازی سبک در AWS با استفاده از Terraform، می‌توانید از ماژول `t2.micro` (که یک نمونه معمولی و کم هزینه از ماشین‌های EC2 است) استفاده کنید. 

در ادامه، یک مثال از چگونگی تعریف و راه‌اندازی یک نمونه `t2.micro` با استفاده از Terraform در AWS آمده است:

1. **فایل `main.tf`**:

```hcl
provider "aws" {
  region = "us-west-2"  # تنظیم منطقه AWS
}

resource "aws_instance" "example" {
  ami             = "ami-0c94855ba95c71c99"  # ID ماشین مجازی پایه (مثلاً Amazon Linux 2)
  instance_type   = "t2.micro"  # نوع نمونه (t2.micro یک اندازه سبک و اقتصادی است)
  key_name        = "my-key-pair"  # نام کلید عمومی EC2
}

output "instance_public_ip" {
  value = aws_instance.example.public_ip
}
```

2. **فایل `variables.tf`**:

```hcl
variable "aws_region" {
  description = "منطقه AWS"
  type        = string
  default     = "us-west-2"
}

variable "key_name" {
  description = "نام کلید عمومی EC2"
  type        = string
  default     = "my-key-pair"
}
```

3. **فایل `terraform.tfvars`**:

```hcl
aws_region = "us-west-2"
key_name = "my-key-pair"
```

4. **راه‌اندازی ماشین مجازی**:

برای اجرای کد Terraform و راه‌اندازی ماشین مجازی، از دستورات زیر استفاده کنید:

```
terraform init
terraform apply
```

با این کد، یک ماشین مجازی با نوع `t2.micro` در منطقه `us-west-2` ایجاد می‌شود.

توجه داشته باشید که قبل از استفاده از این مثال، باید Terraform و ابزارهای مرتبط با آن را نصب کرده و از یک کلید عمومی EC2 برای ورود به ماشین استفاده کنید.

می‌توانید تنظیمات و مشخصات ماشین مجازی را بر اساس نیاز‌های خود تغییر دهید.