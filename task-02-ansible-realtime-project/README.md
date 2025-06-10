

---

# 🚀 Task 02: Ansible Real-Time Project — AWS EC2 Provisioning & Management

This project demonstrates a real-time use case of using Ansible to provision and manage AWS EC2 instances securely. It covers three main tasks:

1. Provisioning an EC2 instance
2. Enabling passwordless SSH access
3. Stopping the EC2 instance

---

## ✅ Task 1: Provision EC2 Instance with Ansible

### 🔹 Step 1: Install Dependencies

Install the required Python SDK and the Amazon AWS Ansible collection:

```bash
pip install boto3
ansible-galaxy collection install amazon.aws
```

---

### 🔹 Step 2: Set Up Vault to Store AWS Credentials

Generate a secure vault password file:

```bash
openssl rand -base64 2048 > vault.pass
```

Create a vault-encrypted file to store your AWS access credentials:

```bash
ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass
```

Inside `pass.yml`, add the following variables:

```yaml
ec2_access_key: YOUR_AWS_ACCESS_KEY
ec2_secret_key: YOUR_AWS_SECRET_KEY
```

---

### 🔹 Step 3: Run the EC2 Creation Playbook

To provision an EC2 instance using the playbook:

```bash
ansible-playbook ec2_create.yaml --vault-password-file vault.pass
```

---

## ✅ Task 2: Set Up Passwordless SSH Access

After creating the EC2 instance, enable passwordless login by copying your SSH key to the instance:

```bash
ssh-copy-id -f "-o IdentityFile <PATH_TO_PEM_FILE>" ubuntu@<INSTANCE_PUBLIC_IP>
```

Verify the passwordless SSH connection:

```bash
ssh ubuntu@<INSTANCE_PUBLIC_IP>
```

If successful, you will be logged in without being prompted for a password.

---

## ✅ Task 3: Stop EC2 Instance with Ansible

Update your `inventory.ini` file to include the public IP or hostname of the target instance.

Then run the following playbook to stop the instance:

```bash
ansible-playbook -i inventory.ini ec2_stop.yaml --vault-password-file vault.pass
```

---

## 🔐 Security Notes

* The `group_vars/all/pass.yml` file is encrypted using `ansible-vault`.
* **Never upload your `vault.pass` file to any public GitHub repository.**
* Ensure your IAM user has appropriate EC2 permissions like:

  * `ec2:RunInstances`
  * `ec2:StopInstances`
  * `ec2:DescribeInstances`

---

