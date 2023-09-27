# erraform kuber provider

با استفاده از Terraform و provider Kubernetes می‌توانید محیط‌های Kubernetes را مدیریت کنید. در اینجا یک مثال ساده برای ایجاد یک deployment در Kubernetes با استفاده از Terraform و provider Kubernetes است:

ابتدا اطمینان حاصل کنید که Terraform و provider Kubernetes روی سیستم شما نصب شده باشد.

بسازید یک پوشه جدید برای پروژه خود و وارد آن شوید.

یک فایل با نام `main.tf` ایجاد کنید و کد زیر را در آن قرار دهید:
provider.tf
```hcl
provider "kubernetes" {
  config_path = "~/.kube/config"  # مسیر فایل تنظیمات Kubernetes
}
```

در اینجا یک منبع از نوع kubernetes_namespace تعریف شده است که یک namespace در Kubernetes ایجاد می‌کند. نام این namespace example-namespace است.
  
در این بخش، یک منبع از نوع kubernetes_deployment تعریف شده است که یک Deployment در Kubernetes ایجاد می‌کند.

نام Deployment تعیین شده در metadata به example-deployment است و متعلق به namespace ایجاد شده در مرحله قبل است.

تعداد replicas برابر با 3 تنظیم شده است.

از لحاظ لیبل‌گذاری، این Deployment بر اساس label app=example-app قابل انتخاب است.

در بخش template تنظیمات پاد کانتینر ایجاد می‌شود که تصویر nginx:latest را اجرا می‌کند.

نام کانتینر تعیین شده به example-container است.

با اجرای این کدها، شما یک namespace به نام example-namespace و یک deployment با نام example-deployment و 3 نسخه از image nginx:latest ایجاد می‌کنید.


main.tf
```hcl
resource "kubernetes_namespace" "example_namespace" {
  metadata {
    name = "example-namespace"
  }
}

resource "kubernetes_deployment" "example_deployment" {
  metadata {
    name      = "example-deployment"
    namespace = kubernetes_namespace.example_namespace.metadata.0.name
  }

  spec {
    replicas = 3

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

4. سپس از طریق ترمینال یا خط فرمان، به پوشه پروژه خود بروید و دستورات زیر را اجرا کنید:

   ```bash
   terraform init
   terraform apply
   ```

ترافورم  سوالات مربوط به متغیرها را پرسیده و سپس تأییدیه ایجاد deployment و namespace را نشان خواهد داد. با تایپ `yes` این تأییدیه‌ها را بدهید.

پس از اجرای موفقیت‌آمیز، Terraform می‌بایست داده‌ها و وضعیت جدید را نمایش دهد.

اکنون یک namespace به نام `example-namespace` و یک deployment به نام `example-deployment` با 3 نسخه از تصویر `nginx:latest` ایجاد شده است.

لطفا توجه داشته باشید که برای اجرای این مثال، شما باید دسترسی به یک کلاستر Kubernetes داشته باشید و تنظیمات آن در فایل `~/.kube/config` قرار داشته باشد.

همچنین اطمینان حاصل کنید که Terraform و provider Kubernetes نصب شده باشند و از ورژن‌های مناسب استفاده می‌کنید.

برای تاییدیه کارهای انجام شده :
```shell
 kubectl get pod --namespace example-namespace
NAME                                  READY   STATUS    RESTARTS   AGE
example-deployment-5d7fd759c8-8xtxt   1/1     Running   0          44m
example-deployment-5d7fd759c8-lcldv   1/1     Running   0          44m
example-deployment-5d7fd759c8-zzhmn   1/1     Running   0          44m
```
```shell
kubectl get ns
NAME                 STATUS   AGE
example-namespace    Active   45m
```
