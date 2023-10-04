# create multi container with variable on docker

```hcl
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


variable "container_names" {
  type    = list(string)
  default = ["container1", "container2"]
}

variable "image_name" {
  type    = string
  default = "nginx:latest"
}

variable "port_mappings" {
  type    = list(string)
  default = ["8080:80", "8081:80"]
}

resource "docker_container" "containers" {
  count = length(var.container_names)

  name  = var.container_names[count.index]
  image = var.image_name

  restart = "always"

  // این قسمت را می‌توانید با تنظیمات دیگر مطابق نیاز خود تغییر دهید
}

```

این کد به شما کمک می‌کند تا با استفاده از Terraform و پرووایدر Docker، دو کانتینر از تصویر `nginx:latest` ایجاد کنید. کد به صورت زیر است:

1. **تعریف پرووایدر و مورد ارتباط با Docker**:

```hcl
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}
```

- در این بخش، پرووایدر Docker تعریف شده است که به Terraform اطلاع می‌دهد که از پرووایدر Docker استفاده می‌کند.

2. **تعریف تصویر Docker**:

```hcl
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}
```

- این بخش تعریف تصویر Docker به نام `nginx:latest` را انجام می‌دهد. این تصویر از منبع اصلی (مثلاً Docker Hub) دانلود خواهد شد.

3. **تعریف متغیرها**:

```hcl
variable "container_names" {
  type    = list(string)
  default = ["container1", "container2"]
}

variable "image_name" {
  type    = string
  default = "nginx:latest"
}

variable "port_mappings" {
  type    = list(string)
  default = ["8080:80", "8081:80"]
}
```

- در این بخش، سه متغیر تعریف شده‌اند: `container_names` برای نام کانتینر‌ها، `image_name` برای نام تصویر و `port_mappings` برای مپینگ پورت‌ها.

4. **ایجاد کانتینرها**:

```hcl
resource "docker_container" "containers" {
  count = length(var.container_names)

  name  = var.container_names[count.index]
  image = var.image_name

  restart = "always"
}
```

- در این بخش، از ماژول `docker_container` برای ایجاد کانتینر‌ها استفاده می‌شود. `count` به تعداد کانتینرها می‌رسد که مقدار آن از طریق طول لیست `container_names` تعیین می‌شود. هر کانتینر با نام و تصویر مشخصی ایجاد می‌شود و همچنین با استفاده از `restart = "always"`، تنظیم می‌شود تا در صورت خاموش شدن، به صورت خودکار راه‌اندازی شود.

با اجرای این کد، دو کانتینر با نام‌ها `container1` و `container2` از تصویر `nginx:latest` ایجاد خواهند شد، و پورت‌های `8080:80` و `8081:80` برای آن‌ها مپ خواهند شد.
