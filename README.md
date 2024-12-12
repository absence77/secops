# secops
Terraform install Aws infra

Launching a system that deploys infrastructure in AWS using Terraform.
It provisions an EC2 instance and installs a Python Docker container on it.

The issue is that during the infrastructure setup, port 443 remains open. NGINX is installed and configured as a reverse proxy to route port 443 to port 22, allowing Ansible from my local machine to run its playbook. The playbook installs the Docker container, retrieves keys from AWS Secrets Manager, and configures them inside the Docker container.
I have a pre-configured NGINX file, and during the Terraform deployment, it is stored in an S3 bucket and then placed on the EC2 instance. This way, using port 443, Ansible can connect to the Docker container and launch any tool or environment as needed.
The most critical step is that after applying the Terraform configuration, the EC2 DNS must be added to the Ansible inventory. Additionally, the playbook should reference the correct secret name from which to retrieve the necessary data.

I used a placeholder SSH key there.
It can be added to .gitignore, passed directly, or set up to read from a specific path. This depends on how the team operates and the established standards. Personally, I would put it in a variable and add it to .gitignore, but I can adjust it if you prefer.

![image](https://github.com/user-attachments/assets/47c76d25-e990-4399-ad6d-2e01e01d3c33)

You can run the following command to execute the Ansible playbook:
ansible-playbook -i inventory.ini docker_container.yml
You can also connect to the machine locally and verify the keys present there:
ssh ec2-user@ec2-18-204-206-23.compute-1.amazonaws.com -p 443

![image](https://github.com/user-attachments/assets/503e3aa7-278a-4253-9487-e4b7adb990a7)

Here are the commands you might need:
sudo docker exec -ti "docker container" /bin/sh
Make sure to select the correct name and set it for the secret manager.

 
