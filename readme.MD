    
# ⚙️ Terraform Module: Ansible Controller for RoboShop

> Provision a ready-to-use Ansible controller to automate backend configuration for the [RoboShop Infrastructure Project](https://github.com/Nallagachu/roboshop-Infra-dev).

---

## 📖 Overview

This Terraform module sets up an Ansible controller instance in the cloud (e.g., AWS EC2) to manage backend component configuration for the RoboShop e-commerce application.

It automates:

- Launching a virtual machine with cloud-init support
- Installing Ansible and Git
- Cloning your RoboShop Ansible roles repository
- Preparing the instance to execute Ansible playbooks (manually or via automation)

> ✅ This module is **consumed as a dependency** by the main infrastructure project: [**roboshop-Infra-dev**](https://github.com/Nallagachu/roboshop-Infra-dev)

---

## 🛠️ Purpose

This module is not standalone. It enables the **backend configuration** layer for RoboShop by:

- Providing a centralized Ansible controller
- Executing component roles like MongoDB, Catalogue, Redis, etc.
- Supporting layered deployment alongside VPC, ALBs, and databases

---

## 📁 Directory Structure

```

ansible-roboshop-roles-tf/
├── main.tf           # Core Terraform logic
├── variables.tf      # Input variable definitions
├── outputs.tf        # Output values (IP, SSH command)
├── user\_data.tpl     # Cloud-init script for bootstrapping Ansible
└── README.md         # You’re reading it now

````

---

## 🔧 Input Variables

| Name                  | Description                                        | Type   | Required |
|-----------------------|----------------------------------------------------|--------|----------|
| `cloud_provider`      | Cloud platform to use (e.g., `aws`)               | string | ✅       |
| `region`              | Region to launch the instance                     | string | ✅       |
| `instance_type`       | Instance size (e.g., `t3.medium`)                 | string | ✅       |
| `ssh_key_name`        | Name of the EC2 key pair for SSH access           | string | ✅       |
| `ansible_roles_source`| Git URL of the Ansible roles repo                 | string | ✅       |
| `ansible_extra_vars`  | Extra variables passed to Ansible playbook        | map    | ❌       |

---

## 📤 Outputs

| Output Name   | Description                                    |
|---------------|------------------------------------------------|
| `public_ip`   | Public IP of the Ansible controller instance   |
| `ssh_user`    | SSH user (e.g., `ec2-user`)                    |
| `ssh_command` | Full SSH command to access the controller      |

---

## 🚀 Usage (from `roboshop-Infra-dev`)

Add the module in your main Terraform project:

```hcl
module "ansible_controller" {
  source                = "github.com/Nallagachu/ansible-roboshop-roles-tf"
  cloud_provider        = "aws"
  region                = "us-east-1"
  instance_type         = "t3.medium"
  ssh_key_name          = "my-key"
  ansible_roles_source  = "git::https://github.com/Nallagachu/roboshop-ansible.git"

  ansible_extra_vars = {
    environment = "dev"
  }
}
````

---

## 🔐 Connect to the Ansible Controller

Once deployed:

```bash
# SSH into the controller
ssh ec2-user@<public_ip> -i ~/.ssh/my-key.pem
```

> The instance will have Ansible installed and roles cloned from your Git source.

---

## 🌐 Linked Repositories

* 🔧 **Infra Automation**: [roboshop-Infra-dev](https://github.com/Nallagachu/roboshop-Infra-dev)
* 📦 **Ansible Roles**: [roboshop-ansible](https://github.com/Nallagachu/roboshop-ansible)

---

## 🤝 Contributions

Feel free to fork this repo, raise issues, or contribute enhancements. Collaboration is welcome!

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](./LICENSE) file for details.

---

## 🙋‍♂️ Maintained By

**Durgababu Nallagachu**
📧 Reach out via [GitHub](https://github.com/Nallagachu) for support or collaboration.

---

## 💡 Final Note

This module is essential to the RoboShop provisioning workflow. By separating the controller logic from the core infrastructure, it promotes modularity, reusability, and clean automation practices.

> 🔍 **Tip**: Use this with `terraform apply` in your root project and watch your backend get configured in minutes!

```

