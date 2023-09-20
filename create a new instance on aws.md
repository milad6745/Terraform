برای ایجاد یک ماشین مجازی سبک در AWS با استفاده از Terraform، می‌توانید از ماژول `t2.micro` (که یک نمونه معمولی و کم هزینه از ماشین‌های EC2 است) استفاده کنید. 

![t2_credits_baseline](https://github.com/milad6745/Terraform/assets/113288076/535cc88f-006a-4ad9-a71c-35b4a09db337)




در ادامه، یک مثال از چگونگی تعریف و راه‌اندازی یک نمونه `t2.micro` با استفاده از Terraform در AWS آمده است:

**فایل `main.tf`**:

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

**فایل `variables.tf`**:
فایل `variables.tf` در Terraform برای تعریف متغیرها (Variables) استفاده می‌شود. متغیرها به شما امکان می‌دهند تا اطلاعاتی را که ممکن است در طول فرآیند اجرای Terraform تغییر کنند را از کد جدا کنید. این اطلاعات می‌توانند شامل اطلاعاتی مانند نام‌های فایل‌ها، نام‌های کلیدها، شماره‌ها و غیره باشند.

فایل `variables.tf` معمولاً به صورت زیر به نظر می‌رسد:

- `description`: توضیحی است که برای متغیر تعیین می‌کند که این متغیر چه کاربردی دارد یا ممکن است چه مقادیری بپذیرد.
- `type`: نوع داده متغیر را مشخص می‌کند (مانند string برای رشته، number برای عدد و غیره).
- `default`: مقدار پیش‌فرضی که این متغیر در صورت عدم تعیین مقدار از سوی کاربر خواهد داشت.

با تعریف متغیرها در فایل `variables.tf`، می‌توانید از آن‌ها در ماژول‌ها و منابع Terraform استفاده کنید. همچنین این امکان را دارید که مقادیر آن‌ها را در فایل `terraform.tfvars` تعریف کنید تا از مقدار‌دهی اولیه مقادیر استفاده شود.

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

**فایل `terraform.tfvars`**:

```hcl
aws_region = "us-west-2"
key_name = "my-key-pair"
```

**راه‌اندازی ماشین مجازی**:

برای اجرای کد Terraform و راه‌اندازی ماشین مجازی، از دستورات زیر استفاده کنید:

```
terraform init
terraform apply
```

با این کد، یک ماشین مجازی با نوع `t2.micro` در منطقه `us-west-2` ایجاد می‌شود.

توجه داشته باشید که قبل از استفاده از این مثال، باید Terraform و ابزارهای مرتبط با آن را نصب کرده و از یک کلید عمومی EC2 برای ورود به ماشین استفاده کنید.

می‌توانید تنظیمات و مشخصات ماشین مجازی را بر اساس نیاز‌های خود تغییر دهید.
