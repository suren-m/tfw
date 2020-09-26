#### Deploy Docker Containers with Terraform

> Note: This requires Docker desktop / engine installed in your environment with sufficient privileges

* Create a `main.tf` in a separate directory with following contents and `cd` into it. 

```json
terraform {
  required_providers {
    docker = {
      source = "terraform-providers/docker"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "demo"
  ports {
    internal = 80
    external = 8000
  }
}

```

* terraform apply

---

#### Verify

* Do a `docker ps`. This should show the nginx container deployed via terraform

* Run a `terraform destroy` and do `docker ps` again to verify that the container is removed.