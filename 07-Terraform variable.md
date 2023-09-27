# Terraform variable
 در زیر یک مثال از چگونگی استفاده از متغیرها در فایل `main.tf` با استفاده از Terraform ارائه می‌شود:

فرض کنید می‌خواهیم یک Deployment در Kubernetes ایجاد کنیم و نیاز داریم تعداد پلیکیشن‌هایی که در این Deployment اجرا می‌شوند، را به عنوان یک متغیر در نظر بگیریم.

**ساخت فایل `variables.tf`:**

   در این فایل، متغیرها را تعریف می‌کنیم. برای مثال، تعداد پلیکیشن‌ها را به عنوان یک متغیر تعریف می‌کنیم.

   ```hcl
   variable "app_count" {
     description = "تعداد پلیکیشن‌ها"
     type        = number
     default     = 3
   }
   ```

   در اینجا، ما یک متغیر به نام `app_count` تعریف کرده‌ایم که از نوع `number` است و مقدار پیش‌فرض آن 3 می‌باشد.

**استفاده از متغیر در فایل `main.tf`:**

   در فایل `main.tf` می‌توانیم از این متغیر استفاده کنیم. به عنوان مثال، در تعریف `kubernetes_deployment`:

   ```hcl
   resource "kubernetes_deployment" "example_deployment" {
     metadata {
       name      = "example-deployment"
       namespace = kubernetes_namespace.example_namespace.metadata.0.name
     }

     spec {
       replicas = var.app_count

       selector {
         match_labels = {
           app = "example-app"
         }
       }

       template {
         metadata {
           labels = {
             app = "example-app"
           }
         }

         spec {
           container {
             image = "nginx:latest"
             name  = "example-container"
           }
         }
       }
     }
   }
   ```

   در اینجا، ما از متغیر `var.app_count` استفاده کرده‌ایم تا تعداد پلیکیشن‌ها را مشخص کنیم.

**استفاده از مقدار متغیر در دستورات:**

   در صورتی که می‌خواهید از این مقدار در دستورات یا محاسبات دیگر استفاده کنید، می‌توانید از `var.app_count` استفاده کنید.

این مثال نشان می‌دهد چگونه می‌توانید از متغیرها در Terraform استفاده کنید تا مقادیر قابل تنظیم و دینامیک در کانفیگ‌های خود داشته باشید.

# Terraform variable example aw
 در زیر یک مثال دیگر از چگونگی استفاده از متغیرها در Terraform ارائه می‌شود:

**ساخت فایل `variables.tf`:**

   در این فایل، متغیرها را تعریف می‌کنیم. در این مثال، ما یک متغیر به نام `instance_type` برای نوع ماشین‌های مجازی در یک ابر فرضی تعریف می‌کنیم.

   ```hcl
   variable "instance_type" {
     description = "نوع ماشین مجازی"
     type        = string
     default     = "t2.micro"
   }
   ```

   در اینجا، ما یک متغیر به نام `instance_type` تعریف کرده‌ایم که از نوع `string` است و مقدار پیش‌فرض آن "t2.micro" می‌باشد.

**استفاده از متغیر در فایل `main.tf`:**

   در فایل `main.tf` می‌توانیم از این متغیر استفاده کنیم. برای مثال، در تعریف یک instance در اینجا:

   ```hcl
   resource "aws_instance" "example_instance" {
     ami           = "ami-0c94855ba95c71c99"
     instance_type = var.instance_type
     count         = 2

     tags = {
       Name = "example-instance"
     }
   }
   ```

   در اینجا، ما از متغیر `var.instance_type` استفاده کرده‌ایم تا نوع ماشین‌های مجازی را مشخص کنیم.

**استفاده از مقدار متغیر در دستورات:**

   می‌توانید مقدار این متغیر را در دستورات دیگر مانند محاسبات یا ارسال آن به سرویس‌ها استفاده کنید.

این مثال نشان می‌دهد چگونه می‌توانید از متغیرها در Terraform استفاده کنید تا مقادیر قابل تنظیم و دینامیک در کانفیگ‌های خود داشته باشید.
