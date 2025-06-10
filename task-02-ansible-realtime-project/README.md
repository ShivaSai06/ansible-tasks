# 🚀 Task 02: Ansible Real-Time Project — AWS EC2 Provisioning & Management

This project demonstrates a real-time use case using Ansible for managing AWS EC2 instances securely with vault-encrypted credentials. It includes three main tasks: provisioning an instance, setting up passwordless SSH, and stopping the instance.

---

## ✅ Task 1: Provision EC2 Instance with Ansible

### 🔹 Step 1: Install Dependencies

Install the required Python SDK and Ansible collection:

```bash
pip install boto3
ansible-galaxy collection install amazon.aws
🔹 Step 2: Set Up Vault to Store AWS Credentials
Generate a vault password file:

bash
Copy
Edit
openssl rand -base64 2048 > vault.pass
Store your AWS access and secret keys in a vault-protected file:

bash
Copy
Edit
ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass
In pass.yml, add:

yaml
Copy
Edit
ec2_access_key: YOUR_AWS_ACCESS_KEY
ec2_secret_key: YOUR_AWS_SECRET_KEY
🔹 Step 3: Run the EC2 Creation Playbook
Use the ec2_create.yaml playbook to provision your EC2 instance:

bash
Copy
Edit
ansible-playbook ec2_create.yaml --vault-password-file vault.pass
✅ Task 2: Set Up Passwordless SSH Access
After your EC2 instance is created, use the following command to enable passwordless login:

bash
Copy
Edit
ssh-copy-id -f "-o IdentityFile <PATH TO PEM FILE>" ubuntu@<INSTANCE-PUBLIC-IP>
Then verify the connection:

bash
Copy
Edit
ssh ubuntu@<INSTANCE-PUBLIC-IP>
✅ If successful, this logs in without prompting for a password.

✅ Task 3: Stop EC2 Instance with Ansible
Define the target instance in inventory.ini.

Use the ec2_stop.yaml playbook to stop the instance:

bash
Copy
Edit
ansible-playbook -i inventory.ini ec2_stop.yaml --vault-password-file vault.pass
🔐 Notes
The group_vars/all/pass.yml file is encrypted using ansible-vault.

Never upload your vault.pass file to a public repository.

Ensure your AWS IAM user has permissions to create and stop EC2 instances.