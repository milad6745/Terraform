دستور `terraform output` در Terraform برای نمایش مقادیر Output های تعریف شده در فایل‌های `output.tf` استفاده می‌شود. Output ها در Terraform برای ارائه اطلاعات اضافی مانند آدرس IP، پورت‌ها و دیگر مشخصات مفید به کاربران پس از اجرای `terraform apply` می‌باشند.

به عنوان مثال، اگر در فایل `output.tf` شما یک خروجی به نام `example_service_ip` تعریف کرده باشید، می‌توانید با استفاده از دستور `terraform output example_service_ip` آدرس IP سرویس را دریافت کنید.

استفاده از دستور `terraform output` به صورت تکی مقدار Output مورد نظر را نمایش می‌دهد. اگر بخواهید تمام مقادیر Output ها را نمایش دهید، می‌توانید از `terraform output` بدون هیچ پارامتر اضافی استفاده کنید.


د اینجا ما با استفاده از output.tf میخواهیم کلاستر آیپی و نود پورت را که با استفاده از ترافورم آپ کردیم به دست آوریم .

برای این کار ابتدا terraform show میزنیم و سپس مقادیری که میخواهیم را داخل output.tf وارد میکنیم .


output.tf
```

output "example_service_ip" {
  value = kubernetes_service.master_svc.spec.0.cluster_ip
}

output "example_service_port" {
  value = kubernetes_service.master_svc.spec.0.port.0.target_port
}

```
مواردی که بصورت لیست است باید .0 داشته باشد.

```hcl
resource "kubernetes_service" "master_svc" {
 spec {
      
        cluster_ip                        = "10.96.179.215

        port {
            node_port   = 0

        }
    }
```

```shell
terraform apply
example_service_ip = "10.96.179.215"
example_service_port = "8080"
```
