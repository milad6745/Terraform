منبع داده (data source) مخصوص پرووایدر Kubernetes در Terraform برای بازیابی اطلاعات از یک کلاستر Kubernetes موجود است. در زیر یک مثال از استفاده از `terraform data` برای بازیابی اطلاعات تعریف شده است:

```hcl
provider "kubernetes" {
  config_path = "~/.kube/config" # مسیر فایل تنظیمات kubeconfig شما
}

data "kubernetes_namespace" "example_namespace" {
  metadata {
    name = "example-namespace"
  }
}

data "kubernetes_service" "example_service" {
  metadata {
    name      = "example-service"
    namespace = data.kubernetes_namespace.example_namespace.metadata.0.name
  }
}
```

در این مثال، ما از `terraform data` استفاده کرده‌ایم تا اطلاعات یک فضای نام (namespace) و یک سرویس Kubernetes را از کلاستر موجود دریافت کنیم.

- در بخش اول، ما اطلاعات یک فضای نام با نام "example-namespace" را با استفاده از منبع داده `kubernetes_namespace` بازیابی کرده‌ایم.

- در بخش دوم، ما اطلاعات یک سرویس Kubernetes با نام "example-service" وابسته به فضای نامی که در بخش قبلی بازیابی کردیم، با استفاده از منبع داده `kubernetes_service` بازیابی می‌کنیم.

این اطلاعات حالا در دسترس متغیرها و می‌توانند در دیگر بخش‌های کد Terraform استفاده شوند.
