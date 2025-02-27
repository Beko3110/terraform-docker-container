# Terraform Docker Container

In the working directory, create a file called main.tf and paste the following Terraform configuration into it.


Mac or Linux or Windows

```terraform
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {
  host    = "npipe:////.//pipe//docker_engine"
}

resource "docker_image" "nginx" {
  name         = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"

  ports {
    internal = 80
    external = 8000
  }
}
```

Initialize the project, which downloads a plugin called a provider that lets Terraform interact with Docker.

```bash
$ terraform init
```
Provision the NGINX server container with apply. When Terraform asks you to confirm type yes and press ENTER.
```bash
$ terraform apply
```
Verify the existence of the NGINX container by visiting localhost:8000 in your web browser or running docker ps to see the container.

NGINX running in Docker via Terraform
```bash
$ docker ps
CONTAINER ID        IMAGE                     COMMAND                  CREATED             STATUS              PORTS                    NAMES
425d5ee58619        e791337790a6              "nginx -g 'daemon of…"   20 seconds ago      Up 19 seconds       0.0.0.0:8000->80/tcp     tutorial
```
To stop the container, run terraform destroy.
```bash
$ terraform destroy
```
You've now provisioned and destroyed an NGINX webserver with Terraform.

![alt text](https://github.com/Beko3110/Terraform-Docker-Container/blob/master/nginx.png?raw=true)