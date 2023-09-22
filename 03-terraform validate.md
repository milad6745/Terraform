# Terraform validate


دستور `terraform validate` در Terraform برای اعتبارسنجی صحت و اعتبار فایل‌های توصیف زیرساخت (مانند فایل‌های `.tf`) در یک پروژه استفاده می‌شود. این دستور بررسی می‌کند که آیا سینتکس و ساختار فایل‌ها صحیح است یا خیر.

مثال:
فرض کنید یک فایل به نام `main.tf` داریم که محتوای زیر را دارد:

```yaml
provider "aws" {
  region = "us-west-2"
}

resource "aws_instance" "example" {
  ami           = "ami-0c94855ba95c71c99"
  instance_type = "t2.micro"
}
```

اگر شما در مسیر پروژه Terraform‌تان باشید و اجرای دستور `terraform validate` را انجام دهید، اگر کلیه فایل‌های `.tf` شما معتبر باشند، خروجیی نداشته باشید و دستور به سرعت اجرا می‌شود.

اما اگر یکی از فایل‌های شما دارای مشکلات نحوی یا ساختاری باشد، پیغام خطا مرتبط با آن فایل نمایش داده خواهد شد.

به طور کلی، `terraform validate` یک ابزار مفید است برای اطمینان حاصل کردن از صحت فایل‌های موجود در پروژه Terraform قبل از اجرای هرگونه عملیاتی مانند `terraform plan` یا `terraform apply`.


## check validation
```shell
# terraform init
erraform has been successfully initialized!

# terraform validate
Success! The configuration is valid.
```
