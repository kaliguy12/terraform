After helping your friend deploying their Ghost blog using Terraform, you’ve experienced an increase in demand for the product. To make deploy Ghost blog easier, you’ve decided to convert your existing Terraform files into a module.

Create Ghost module
Create a directory called ghost.
Your modules will be made up of three files: main.tf, variables.tf and outputs.tf.

main.tf
In main.tf you will deploy out two resources docker_image and docker_container. The docker_image resource name will be ghost_image.
The name will use th image_name variable.
The docker_container resource name will be ghost_container.
The name will be set using a varialbe called container_name. The image will be set usingdocker_image.ghost_image.latest. Set the external port to use the ext_port variable.

variables.tf
In variables.tf create three varialbes: image_name, container_name and ext_port.

outputs.tf
In outputs.tf create two outputs: ip and container_name. The ip output the ghost_container's ip_address attribute.
The container_name output the ghost_container's name attribute.

Create root module
main.tf
In user directory create main.tf, variables.tf and outputs.tf. In main.tf will use the ghost module.
Set image_name using a variable called image_name.
Set container_name using a variable called container_name.
Set ext_port using a variable called ext_port.
In variables.tf create three image_name, container_name and ext_port.

variables.tf
The image_name will have a default value of ghost:latest with a description of Image for container.
The container_name will have a default value of blogwith a description ofName of the container.  
Theext_portwill have adefaultvalue of 80 with a description of External port for container.

outputs.tf
In outputs.tf create two outputs: ip and container_name. The ip output the ghost_container's ip_address attribute.
The container_name output the ghost_container's name attribute.

Deploy the infrastructure
Initialize Terraform.
terraform init
Generate a Terraform plan and output a plan file.
terraform plan -out=tfplan -var image_name=ghost:alpine -var ext_port=8080
Deploy the infrastructure using the plan file.
terraform apply tfplan

task
Create the ghost directory:

mkdir ghost
cd ghost
Create main.tf:

vi main.tf

resource "docker_image" "ghost_image" {
  name = "${var.image_name}"
}

# Start the Ghost Container
resource "docker_container" "ghost_container" {
  name  = "${var.container_name}"
  image = "${docker_image.ghost_image.latest}"
  ports {
    internal = "2368"
    external = "${var.ext_port}"
  }
}
Create variables.tf:

vi variables.tf

#Define variables
variable "image_name" {}
variable "container_name" {}
variable "ext_port" {}
Create outputs.tf:

vi outputs.tf

#Output the IP Address of the Container
output "ip" {
  value = "${docker_container.ghost_container.ip_address}"
}

output "container_name" {
  value = "${docker_container.ghost_container.name}"
}

Create main.tf: Change back to the user directory.

cd ~/
vi main.tf

module "ghost" {
  source             = "./ghost"
  image_name         = "${var.image_name}"
  container_name     = "${var.container_name}"
  ext_port           = "${var.ext_port}"
}
Create variables.tf:

vi variables.tf

#Define variables
variable "image_name" {
  description = "Image for container."
  default     = "ghost:latest"
}

variable "container_name" {
  description = "Name of the container."
  default     = "blog"
}

variable "ext_port" {
  description = "External port for container."
  default     = "80"
}
Create outputs.tf:

vi outputs.tf

#Output the IP Address of the Container
output "ip" {
  value = "${module.ghost.ip}"
}

output "container_name" {
  value = "${module.ghost.container_name}"
}
