با استفاده از `nodePort` در Kubernetes می‌توانید یک سرویس را به یک پورت خاص در تمامی کلاسترهای کوبرنتیز متصل کنید. در اینجا یک مثال استفاده از
`nodePort` در Terraform با provider Kubernetes:

فایل `main.tf`:

```hcl
provider "kubernetes" {
  config_path = "~/.kube/config"  # مسیر فایل تنظیمات kubeconfig شما
}

resource "kubernetes_namespace" "example" {
  metadata {
    name = "example-namespace"
  }
}

resource "kubernetes_pod" "master" {
  metadata {
    name      = "master-pod"
    namespace = kubernetes_namespace.example.metadata.0.name
  }

  spec {
    container {
      image = "your-master-image"  # تصویر مستر مورد نظر
      name  = "master-container"
    }
  }
}

resource "kubernetes_service" "master_svc" {
  metadata {
    name      = "master-service"
    namespace = kubernetes_namespace.example.metadata.0.name
  }

  spec {
    selector = {
      app = kubernetes_pod.master.metadata.0.name
    }

    port {
      port        = 80  # پورت مورد نظر
      target_port = 80
    }
  }
}
```



در این مثال، `type` سرویس به `NodePort` تغییر داده شده است و `node_port` تنظیم شده است. در این حالت، سرویس بر روی یک پورت خاص در تمام کلاسترها می‌استقرار یابد که از طریق آن می‌توانید به سرویس دسترسی پیدا کنید.

اگر تمایل به تغییرات دیگری داشتید، می‌توانید مقادیر دیگری نظیر نام‌ها و پورت‌ها را در فایل `main.tf` تغییر دهید.

## Verify SVC Create

برای دیدن سرویس‌های ایجاد شده در Kubernetes می‌توانید از دستور kubectl استفاده کنید. ابتدا باید مطمئن شوید که دسترسی به کلاستر Kubernetes خود دارید.

برای دیدن تمامی سرویس‌ها در یک فضای نام خاص، می‌توانید از دستور زیر استفاده کنید

```

└─# kubectl get services -n example-namespace
NAME             TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
master-service   ClusterIP   10.96.7.62   <none>        80/TCP    3m40s

└─# kubectl describe service master-service -n example-namespace
Name:              master-service
Namespace:         example-namespace
Labels:            <none>
Annotations:       <none>
Selector:          app=master-pod
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.96.7.62
IPs:               10.96.7.62
Port:              <unset>  80/TCP
TargetPort:        80/TCP
Endpoints:         <none>
Session Affinity:  None
Events:            <none>
```
