# terraform deploy docker container
در اینجا گفتیم که با استفاده از ترافورم یک کانتینر داکری nginx آپ کن که پورت ۸۰ رو به ۸۰۰۱ مپ کنه .

```yaml
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"
  ports {
    internal = 80
    external = 8001
  }
}
```

```shell
terraform  init

terraform apply
Plan: 2 to add, 0 to change, 0 to destroy.

docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                  NAMES
f15b34c7b070   021283c8eb95   "/docker-entrypoint.…"   4 seconds ago   Up 3 seconds   0.0.0.0:8000->80/tcp   tutorial
```




حالا در صورتی که بخواهیم تغییری در کانتیر مورد نظر ایجاد کنیم قایل main.tf را تغییر داده و سپس مجدد terraform apply میکنیم .
که میبینی که کانتینر ایجاد شده را پاک کرده و سپس کانتینر جدید را با ویرایش درخواستی آپ میکند

```bash
terraform apply
~ ports {
          ~ external = 8000 -> 8002 # forces replacement
apply complete! Resources: 1 added, 0 changed, 1 destroyed.

d‍‍‍‍‍‍‍‍ocker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                  NAMES
834281c6beb5   021283c8eb95   "/docker-entrypoint.…"   7 seconds ago   Up 6 seconds   0.0.0.0:8002->80/tcp   tutorial
```
