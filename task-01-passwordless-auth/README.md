---

# Ansible Passwordless Authentication with AWS EC2

This project demonstrates how to set up Ansible passwordless SSH authentication between a control node (local machine) and two AWS EC2 instances (managed nodes).

---

## Project Overview

* **Control Node**: Local machine (Mac/Linux) with Ansible installed.
* **Managed Nodes**: Two Ubuntu EC2 instances.
* **Goal**: Configure SSH key-based authentication so that Ansible can run commands on the EC2 instances without prompting for a password.

---

### STEP 1: CONTROL NODE SETUP (LOCAL MACHINE)

* Installed Ansible on my local laptop.
* This machine acts as the Ansible control node.

---

### STEP 2: CREATE MANAGED NODES (EC2 INSTANCES)

* Launched 2 Ubuntu EC2 instances from AWS Console:

  * `manage-node-1`
  * `manage-node-2`

---

### STEP 3: PASSWORDLESS SSH FOR `manage-node-1` USING PEM FILE

From the control node, run:

```
ssh-copy-id -f "-o IdentityFile=<PATH-TO-PEM-FILE>" ubuntu@<MANAGE-NODE-1-PUBLIC-IP>
```

Verify the connection:

```
ssh ubuntu@<MANAGE-NODE-1-PUBLIC-IP>
```

✅ You should be able to connect **without entering a password**.

---

### STEP 4: PASSWORD-BASED SSH SETUP FOR `manage-node-2` (ALTERNATIVE METHOD)

Useful if `ssh-copy-id` doesn't work with PEM or you prefer password-based auth.

Connect using PEM:

```
ssh -i <PATH-TO-PEM-FILE> ubuntu@<MANAGE-NODE-2-PUBLIC-IP>
```

Edit SSH config file:

```
sudo vim /etc/ssh/sshd_config.d/60-cloudimg-settings.conf
```

Update the setting:

```
PasswordAuthentication yes
```

Also edit:

```
sudo vim /etc/ssh/sshd_config
```

Uncomment this line:

```
#PasswordAuthentication yes
```

Restart SSH service:

```
sudo systemctl restart ssh
```

Set a password for `ubuntu` user:

```
sudo passwd ubuntu
```

Then from the control node:

```
ssh-copy-id ubuntu@<MANAGE-NODE-2-PUBLIC-IP>
```

Enter password when prompted. Then test:

```
ssh ubuntu@<MANAGE-NODE-2-PUBLIC-IP>
```

✅ You should connect without a password.

---

### STEP 5: CREATE ANSIBLE INVENTORY FILE

Create a file named `inventory.ini`:

```
[app]
ubuntu@<MANAGE-NODE-1-PUBLIC-IP>

[db]
ubuntu@<MANAGE-NODE-2-PUBLIC-IP>
```

---

### STEP 6: TEST CONNECTION WITH ANSIBLE

Ping all managed nodes:

```
ansible -i inventory.ini -m ping all
```

Ping specific groups:

```
ansible -i inventory.ini -m ping app
ansible -i inventory.ini -m ping db
```

✅ Sample Output:

```
ubuntu@<IP> | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

---

