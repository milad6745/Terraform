با استفاده از `nodePort` در Kubernetes می‌توانید یک سرویس را به یک پورت خاص در تمامی کلاسترهای کوبرنتیز متصل کنید. در اینجا یک مثال استفاده از
`nodePort` در Terraform با provider Kubernetes:

فایل `main.tf`:

```hcl
provider "kubernetes" {
  config_path = "~/.kube/config"  # مسیر فایل تنظیمات Kubernetes
}
# ایجاد name space به نام Example
resource "kubernetes_namespace" "example_namespace" {
  metadata {
    name = "example-namespace"
  }
}

# ایجاد سرویس به نام Example
resource "kubernetes_service" "example_service" {
  metadata {
    name      = "example-service"
    namespace = kubernetes_namespace.example_namespace.metadata.0.name
  }

# تعاریف پورت های سرویس
  spec {
    selector = {
      app = kubernetes_deployment.example_deployment.spec.0.template.0.metadata.0.labels.app
    }

    port {
      protocol   = "TCP"
      port       = 80
      target_port = 80
    }

# مدل سرویس ساخته شده
    type      = "NodePort"
    node_port = 30000  # پورت مورد نظر برای دسترسی به سرویس از بیرون کلاستر
  }
}
```


در این مثال، `type` سرویس به `NodePort` تغییر داده شده است و `node_port` تنظیم شده است. در این حالت، سرویس بر روی یک پورت خاص در تمام کلاسترها می‌استقرار یابد که از طریق آن می‌توانید به سرویس دسترسی پیدا کنید.

اگر تمایل به تغییرات دیگری داشتید، می‌توانید مقادیر دیگری نظیر نام‌ها و پورت‌ها را در فایل `main.tf` تغییر دهید.
