We’ve been asked to deploy a MySQL container to the dev environment. with the following requirements:

We must use Terraform.
The container needs to be on a private network.
The MySQL data should persist on its own volume.

Create the variables file
Create variables.tf and add four variables with these default values:
container_name: mysql.
mysql_root_password: P4sSw0rd0!.
mysql_network_name: mysql_internal_network.
mysql_volume_name: mysql_data.
Create the images file
Create images.tf.
Add the docker_image resource and call it mysql_image.
Set the name to mysql:5.7.
Create the networks file
Create networks.tf.
Add the docker_network resource and call it private_bridge_network.
Set the name to use the mysql_network_name variable.
Set the driver to bridge.
Set internal to true.
Create the volumes file
In volumes.tf add the docker_volume resource and call it mysql_data_volume.
Set the name to use the mysql_volume_name variable.
Create the main file
In main.tf add the docker_container resource and call it mysql_container.
Set the name to use the container_name variable.
Set the image to use the name of the image coming from docker_image.
Create an environment variable for MYSQL_ROOT_PASSWORD and set it to the mysql_root_password variable.
Configure the container volume to use the volume created by docker_volume, and make sure the container_path is set to /var/lib/mysql.
The container needs to use the network created by docker_network.
Deploy the infrastructure
Initialize Terraform.
terraform init
Validate the files.
terraform validate
Generate a Terraform plan
terraform plan -out=tfplan.
Deploy the infrastructure using the plan file.
terraform apply tfplan
