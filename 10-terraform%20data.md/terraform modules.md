با استفاده از Terraform و پرووایدر Kubernetes می‌توانید مواردی مانند Deployment و Service را در Kubernetes مدیریت کنید. در این مثال، ما یک فایل Terraform ایجاد می‌کنیم که یک Deployment و یک Service در Kubernetes ایجاد می‌کند و از یک ماژول برای تمیز نگه داشتن کد استفاده می‌کنیم.

1. **ساخت ماژول**:

   ابتدا یک پوشه به نام `k8s_module` ایجاد کنید. در این پوشه، یک فایل با نام `main.tf` ایجاد کنید.

   ```hcl
   # k8s_module/main.tf
   provider "kubernetes" {
     config_context_auth_info = "minikube"
   }

   module "example" {
     source = "../"

     namespace = "default"
     name      = "example"
   }
   ```

   در این فایل، ما از پرووایدر Kubernetes استفاده می‌کنیم و یک ماژول به نام `example` را فراخوانی می‌کنیم.

2. **ایجاد ماژول**:

   در دایرکتوری اصلی پروژه، یک فایل با نام `main.tf` ایجاد کنید که محتوای زیر را داشته باشد:

   ```hcl
   # main.tf
   module "k8s_module" {
     source = "./k8s_module"
   }
   ```

   در این فایل، ما ماژول `k8s_module` را فراخوانی کرده‌ایم که در دایرکتوری `k8s_module` قرار دارد.

3. **ساخت ماژول Kubernetes**:

   در دایرکتوری `k8s_module` یک فایل با نام `k8s_resources.tf` ایجاد کنید.

   ```hcl
   # k8s_module/k8s_resources.tf
   resource "kubernetes_deployment" "example" {
     metadata {
       name = var.name
       namespace = var.namespace
     }

     spec {
       replicas = 2

       selector {
         match_labels = {
           App = var.name
         }
       }

       template {
         metadata {
           labels = {
             App = var.name
           }
         }

         spec {
           container {
             image = "nginx:1.16"
             name  = "example"
           }
         }
       }
     }
   
     resource "kubernetes_service" "example" {
       metadata {
         name = var.name
         namespace = var.namespace
       }
   
       spec {
         selector = {
           App = var.name
         }
   
         port {
           port        = 80
           target_port = 80
         }
   
         type = "LoadBalancer"
       }
     }
   }
   ```

   در این فایل، ما یک Deployment و یک Service برای کانتینر Nginx ایجاد می‌کنیم.

4. **متغیرها و ورودی‌ها**:

   در دایرکتوری `k8s_module` یک فایل با نام `variables.tf` ایجاد کنید.

   ```hcl
   # k8s_module/variables.tf
   variable "name" {
     description = "The name of the resources"
   }

   variable "namespace" {
     description = "The namespace in which to create the resources"
   }
   ```

5. **استفاده از ماژول**:

   حالا می‌توانید با اجرای دستور `terraform init` و سپس `terraform apply`، ماژول Kubernetes را اجرا کنید.

این مثال نشان می‌دهد که چگونه می‌توانید از ماژول‌ها در Terraform استفاده کنید تا کد را بهبود دهید و قابلیت اطمینان و قابلیت استفاده مجدد را افزایش دهید.
