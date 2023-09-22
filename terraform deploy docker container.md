# terraform deploy docker container
در اینجا گفتیم که با استفاده از ترافورم یک کانتینر داکری nginx آپ کن کخ پورت ۸۰ رو به ۸۰۰۱ مپ کنه .
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

‍‍‍‍‍‍‍‍
