دستور `terraform show` یکی از دستورات مهم در Terraform است. این دستور برای نمایش و نمایش مشخصات و وضعیت فعلی محیطی که با Terraform مدیریت می‌شود، استفاده می‌شود. با اجرای این دستور، اطلاعاتی در مورد وضعیت زیرساخت‌ها و منابع در فرمتی خوانا نمایش داده می‌شود.

مثلاً اگر شما با دستور `terraform show` اجرا کنید، ممکن است یک خروجی مانند زیر را ببینید:

```hcl
# kubernetes_namespace.example:
resource "kubernetes_namespace" "example" {
    id                               = "example-namespace"
    wait_for_default_service_account = false

    metadata {
        annotations      = {}
        generation       = 0
        labels           = {}
        name             = "example-namespace"
        resource_version = "195654"
        uid              = "fbabd1f2-94f6-4db0-aa4f-652adbb08dd2"
    }
}

# kubernetes_pod.master:
resource "kubernetes_pod" "master" {
    id = "example-namespace/master-pod"

    metadata {
        annotations      = {}
        generation       = 0
        labels           = {}
        name             = "master-pod"
        namespace        = "example-namespace"
        resource_version = "195670"
        uid              = "c0c2ce04-608f-4200-a45d-487fccd30987"
    }

    spec {
        active_deadline_seconds          = 0
        automount_service_account_token  = true
        dns_policy                       = "ClusterFirst"
        enable_service_links             = true
        host_ipc                         = false
        host_network                     = false
        host_pid                         = false
        node_name                        = "kind-control-plane"
        node_selector                    = {}
        restart_policy                   = "Always"
        scheduler_name                   = "default-scheduler"
        service_account_name             = "default"
        share_process_namespace          = false
        termination_grace_period_seconds = 30

        container {
            args                       = []
            command                    = []
            image                      = "nginx"
            image_pull_policy          = "Always"
            name                       = "master-container"
            stdin                      = false
            stdin_once                 = false
            termination_message_path   = "/dev/termination-log"
            termination_message_policy = "File"
            tty                        = false

            resources {
                limits   = {}
                requests = {}
            }
        }
    }
}

# kubernetes_service.master_svc:
resource "kubernetes_service" "master_svc" {
    id                     = "example-namespace/master-service"
    status                 = [
        {
            load_balancer = [
                {
                    ingress = []
                },
            ]
        },
    ]
    wait_for_load_balancer = true

    metadata {
        annotations      = {}
        generation       = 0
        labels           = {}
        name             = "master-service"
        namespace        = "example-namespace"
        resource_version = "195675"
        uid              = "99af1c6b-56a2-4f12-bc1e-3aad0eb607db"
    }

    spec {
        allocate_load_balancer_node_ports = true
        cluster_ip                        = "10.96.179.215"
        cluster_ips                       = [
            "10.96.179.215",
        ]
        external_ips                      = []
        health_check_node_port            = 0
        internal_traffic_policy           = "Cluster"
        ip_families                       = [
            "IPv4",
        ]
        ip_family_policy                  = "SingleStack"
        load_balancer_source_ranges       = []
        publish_not_ready_addresses       = false
        selector                          = {
            "app" = "master-pod"
        }
        session_affinity                  = "None"
        type                              = "ClusterIP"

        port {
            node_port   = 0
            port        = 80
            protocol    = "TCP"
            target_port = "8080"
        }
    }
}
```
