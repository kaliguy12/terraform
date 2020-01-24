Description
We have a set of preexisting Terraform files, used for deploying our Ghost blog that is in production. Now we want to use these files for deploying out both a production and development version of the blog. To do this, we need to add a new variable called env, convert our existing variables over to maps, and then use a lookup to reference the variables. Afterward, we'll set up two workspaces, dev and prod, where we will deploy our infrastructure.


Complete the Terraform Files
keyboard_arrow_up
Edit variables.tf and add the env variable and convert image_name, container_name and ext_port to maps

In this directory you will find main.tf, outputs.tf, and variables.tf.
And a new variable called env. Set a description to “env: dev or prod”.
Convert the type from image_name to map.

Change the default to use key/value pairs. Set dev to ghost:latest and prod to ghost:alpine.

Convert container_name to a map. Change the default to use key/value pairs. Set dev to blog_dev and prod to blog_prod.

Convert ext_port to a map. Change the default to use key/value pairs. Set dev to 8080 and prod to 80.

Now initialize Terraform.
terraform init

Set up the `dev` Workspace and Deploy the Environment
Create a dev workspace:
terraform workspace new dev

Create a plan for the development deploy:
terraform plan -out=tfdev_plan -var 'env=dev'

Apply the development plan:
terraform apply tfdev_plan

Set up the `prod` Workspace and Deploy the Environment
Create a prod workspace:
terraform workspace new prod

Create a plan for the production deploy:
terraform plan -out=tfprod_plan -var 'env=prod'

Apply the production plan:
terraform apply tfprod_plan
