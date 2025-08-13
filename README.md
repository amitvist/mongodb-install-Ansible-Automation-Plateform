
# MongoDB Installation Playbook via Ansible Automation Platform

This repository contains an **Ansible playbook** designed for installing and configuring **MongoDB** on Linux hosts using **Ansible Automation Platform (AAP)**. It ensures that MongoDB is installed from official repositories, configured securely, and the MongoDB service is started and running.

The playbook supports the installation of MongoDB on **Ubuntu/Debian** and **RHEL/CentOS/Oracle Linux** systems.

---

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Repository Structure](#repository-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Customization](#customization)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

This repository provides an **Ansible playbook** for installing MongoDB. The key functionalities are:

- Install **MongoDB** from official MongoDB repositories (for supported versions).
- Configure MongoDB to **bind to all network interfaces** (or you can specify IP addresses).
- Optionally enable **authentication** to secure MongoDB.
- Ensure **MongoDB** starts automatically and is running.
- Compatible with **Ubuntu**, **Debian**, and **RHEL/CentOS/Oracle Linux**.

---

## Prerequisites

Before running the playbook, ensure you have the following:

1. **Ansible** installed on the system running the automation:
   - To install Ansible, you can use `pip`:
     ```bash
     pip install ansible
     ```

2. **SSH access** to the target Linux host(s) where MongoDB will be installed.
   - Ensure you can connect to the host via SSH.

3. **Root (sudo)** privileges on the target host(s) where MongoDB will be installed.

4. A **supported operating system**: 
   - Ubuntu 20.04, 22.04, or 24.04
   - RHEL 8/9, CentOS 8/9, Oracle Linux 8/9

---

## Repository Structure

The repository has the following structure:

```plaintext
mongodb-installation-repo/
├── playbooks/
│   └── install_mongodb.yml        # Main playbook to install MongoDB
├── files/
│   └── mongod.conf.j2             # Template file for MongoDB configuration (optional)
├── requirements.yml              # Required Ansible collections (community.mongodb)
├── .gitignore                    # Git ignore file
└── README.md                     # This README file
```

### Key Files

- **install_mongodb.yml**: The main playbook that installs MongoDB and ensures it runs.
- **mongod.conf.j2**: A Jinja2 template used to configure MongoDB (enables authentication and sets IP binding).
- **requirements.yml**: Specifies the required Ansible collections (`community.mongodb`).

---

## Installation

### 1. Clone the Repository

To use the playbook, clone this repository to your local machine or server:

```bash
git clone https://github.com/amitvist/mongodb-install-Ansible-Automation-Plateform.git
cd mongodb-install-Ansible-Automation-Plateform
```

### 2. Install Required Ansible Collections

The playbook uses the `community.mongodb` collection to manage MongoDB users and configurations. Install it using the following command:

```bash
ansible-galaxy install -r requirements.yml
```

This will ensure that the necessary collection is available for use in the playbook.

### 3. Set Up Your Inventory

An **inventory** file lists the systems where the playbook will be executed. You can either use an existing inventory or create one in your project directory.

Example inventory (`inventory.yml`):

```yaml
all:
  hosts:
    mongo_host:
      ansible_host: <target-ip-address>
      ansible_user: <your-ssh-user>
      ansible_become: yes
```

Replace `<target-ip-address>` and `<your-ssh-user>` with actual values.

---

## Usage

### 1. Run the Playbook

To run the playbook, use the following command:

```bash
ansible-playbook -i inventory.yml playbooks/install_mongodb.yml
```

This will:
- Install MongoDB on the target host.
- Start the MongoDB service.
- Optionally, configure MongoDB for authentication (if configured in the `mongod.conf.j2` template).

### 2. Verify MongoDB Installation

After the playbook finishes executing, check that MongoDB is installed and running with:

```bash
systemctl status mongod
```

You should see that the MongoDB service is active and running.

### 3. Connect to MongoDB

If you have enabled authentication, you can connect to MongoDB using `mongosh` (MongoDB's shell):

```bash
mongosh "mongodb://<admin-user>:<admin-password>@localhost:27017/admin"
```

Replace `<admin-user>` and `<admin-password>` with the credentials you created or configured.

---

## Customization

You can customize the playbook and MongoDB installation process to suit your needs. Here are a few ways to do so:

### 1. Enable Authentication

By default, MongoDB is installed without authentication. To enable authentication, modify the `mongod.conf.j2` template as follows:

```jinja
security:
  authorization: enabled
```

This will enable **authorization** when MongoDB starts.

### 2. Customize MongoDB Version

The playbook installs MongoDB version 8.x by default. If you need a different version, change the `mongo_major` variable in the playbook:

```yaml
vars:
  mongo_major: "5.0"  # For MongoDB version 5.x
```

### 3. Configure Bind IP

The default configuration binds MongoDB to all IP addresses (`0.0.0.0`), which you can change in the `mongod.conf.j2` template. For example:

```jinja
net:
  bindIp: 127.0.0.1  # or specify a custom IP
```

### 4. MongoDB Replica Set or Sharded Cluster

You can modify the playbook to configure a **replica set** or a **sharded cluster** by adding the appropriate MongoDB configuration in the `mongod.conf.j2` file.

---

## Contributing

We welcome contributions to this repository! Here’s how you can help:

1. Fork this repository.
2. Create a new branch for your changes.
3. Make your changes and test them.
4. Create a pull request with a clear description of your changes.

If you encounter any issues or have suggestions for improvements, feel free to open an issue.

---

## License

This project is licensed under Amit Yadav.
