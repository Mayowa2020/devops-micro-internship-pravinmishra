# Assignment 5 — Deploy EpicBook Web App on Azure VM with Azure Database for MySQL

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will deploy the EpicBook web application on Azure using an Ubuntu Virtual Machine to host the frontend and backend, and Azure Database for MySQL Flexible Server (private access) to store user and product data. You will build the network, provision the resources, deploy the application, and prove that the complete user flow works through the VM's public IP.

---

# Task 1 — Create Network Infrastructure

## Goal

Create a VNet (10.0.0.0/16) with a public subnet (10.0.1.0/24) for the VM and a private subnet (10.0.2.0/24) for MySQL, with NSGs allowing HTTP (80)/SSH (22) publicly and MySQL (3306) only from the VM subnet, plus a Public IP and Network Interface for the VM.

### Evidence

#### Screenshot 1 — Virtual Network overview showing the 10.0.0.0/16 address space and both subnets

![Azure Portal virtual network page for epicbook-vnet showing the address space 10.0.0.0/16 and two subnets, one public and one private, within resource group epicbook-rg in South Africa North. The main subject is the network overview panel, which lists the VNet name, address space, subnet count, and Azure provided DNS service. The wider environment is the Azure Portal management interface, with a calm administrative tone. Visible text includes epicbook-vnet, 10.0.0.0/16, 2 subnets, epicbook-rg, South Africa North, and Azure provided DNS service.](screenshots/week-07-screenshot-14.png)

---

#### Screenshot 2 — Public and private NSG inbound rules showing ports 80, 22, and restricted 3306 access

![Microsoft Azure Portal inbound security rules page for epicbook-public-nsg with the Allow-MySQL-From-Public-Subnet rule selected. The primary subject is the firewall rule table showing ports 80, 22, and 3306 with allowed traffic for HTTP and SSH from the internet and restricted MySQL access. The wider environment is the Azure networking management dashboard, with a technical and administrative tone. Visible text includes epicbook-public-nsg, Inbound security rules, Allow-MySQL-From-Public-Subnet, Priority, Name, Port, Protocol, Source, Destination, Action, 80, 22, 3306, and Allow.](screenshots/week-07-screenshot-15.png)

---

#### Screenshot 3 — Public IP and Network Interface association for the Virtual Machine

![Azure Portal page for the EpicBook virtual machine showing the public IP and network interface resources. The main subject is the VM overview with the public subnet connection and associated networking resources, illustrating that the VM is linked to a public IP and a NIC. The wider environment is the Azure management dashboard, with a calm technical tone. Visible text includes EpicBook VM, Public IP, Network Interface, and related resource names.](screenshots/week-07-screenshot-16.png)

---

# Task 2 — Provision Azure Virtual Machine

## Goal

Launch an Ubuntu 22.04 LTS VM (Standard B1s or equivalent) in the public subnet, and install Node.js, npm, Nginx, Git, and MySQL Client.

### Evidence

#### Screenshot 4 — Virtual Machine overview showing Ubuntu, size, public IP, and subnet

![Azure Portal virtual machine overview for the EpicBook Ubuntu server showing the operating system, VM size, public IP address, and subnet assignment. The main subject is the VM details panel, which confirms Ubuntu 24.04 LTS running on a Standard B1s instance in the public subnet. The wider environment is the Azure resource dashboard, with a routine administrative tone. Visible text includes Ubuntu 24.04 LTS, Standard B1s, Public IP, and subnet details.](screenshots/week-07-screenshot-17.png)

---

#### Screenshot 5 — Terminal showing successful software installation or installed-version checks

![Linux terminal session on the Azure VM showing successful installation and version checks for Node.js, npm, Nginx, Git, and the MySQL client. The primary subject is the command output confirming each package is installed and reporting version numbers, while the shell prompt sits in the wider cloud VM console. The emotional tone is technical and successful. Visible text includes package names and version numbers such as Node.js, npm, Nginx, Git, and MySQL client.](screenshots/week-07-screenshot-18.png)

---

# Task 3 — Deploy the EpicBook Application

## Goal

Clone the EpicBook repository, install dependencies, build the frontend, configure Nginx to serve it, and configure the Node.js/Express.js backend to connect to MySQL using environment variables.

### Evidence

#### Screenshot 6 — Terminal showing the EpicBook repository cloned and dependencies installed

![Ubuntu terminal session in the Azure VM displaying git clone and npm install commands for the EpicBook repository. The main subject is the deployment workflow as the app source code and dependencies are downloaded and installed, while the wider environment is a cloud command-line shell. The tone is technical and successful. Visible text includes repository commands and installation output related to cloning and dependency setup.](screenshots/week-07-screenshot-19.png)

---

#### Screenshot 7 — Nginx configuration or service status proving the frontend is configured to be served

![Linux terminal showing the Nginx configuration and service status output for the EpicBook frontend on the Azure VM. The main subject is the server configuration file and the active status check confirming Nginx is running and serving the app. The wider environment is a cloud server console with a technical operational tone. Visible text includes nginx, server, listen, and active running.](screenshots/week-07-screenshot-20.png)

---

#### Screenshot 8 — Backend process or listening-port evidence (without exposing environment-variable secrets)

![Linux terminal output showing the EpicBook backend process status and listening port information for the Node.js service. The main subject is the running app process and the port it listens on, confirming the backend is active and ready to receive requests. The wider environment is a cloud server console with a calm operational tone. Visible text includes process status details and port information.](screenshots/week-07-screenshot-21.png)

---

# Task 4 — Setup Azure Database for MySQL

## Goal

Create a private Azure Database for MySQL Flexible Server (VNet Integration) in the private subnet, create the database user and schema, import the SQL dump, and restrict access to the VM subnet only.

### Evidence

#### Screenshot 9 — MySQL Flexible Server overview showing Private access (VNet Integration)

![Azure Portal overview of the Azure Database for MySQL Flexible Server configured with private access through virtual network integration. The primary subject is the database server dashboard showing the server name, location, and networking status, while the wider environment is the Azure control plane with a calm administrative tone. Visible text includes the server name, location, and private access information.](screenshots/week-07-screenshot-22.png)

---

#### Screenshot 10 — Networking configuration showing the private subnet and restricted access

![Azure portal networking page for the MySQL server showing the private subnet and restricted access settings so only the VM subnet can reach port 3306. The UI includes network configuration and access controls in a standard Azure management dashboard. The tone is technical and administrative. Visible text includes subnet configuration, virtual network integration, network access, and port 3306.](screenshots/week-07-screenshot-23.png)

---

#### Screenshot 11 — MySQL Client output showing the EpicBook database or imported tables (no password visible)

![MySQL client terminal in the Azure VM showing the EpicBook database and imported tables after the schema has been loaded. The primary subject is the command output listing databases and tables, with no password visible, while the wider environment is a secure Linux shell with a technical and successful tone. Visible text includes database names and table names such as author, book, cart, cartbook, and checkout.](screenshots/week-07-screenshot-24.png)

---

# Task 5 — Test End-to-End Functionality

## Goal

Confirm the EpicBook application loads through the VM's public IP and that viewing products, adding items to the cart, and placing orders all work.

### Evidence

#### Screenshot 12 — Browser showing the EpicBook application with the Virtual Machine public IP visible

![Web browser showing the EpicBook application loaded through the VM public IP, with the homepage visible and the address bar or page title confirming the live Azure deployment. The main subject is the running web app, while the wider environment is a desktop browser window on a standard laptop or workstation screen. The tone is successful and functional. Visible text includes the EpicBook page and the public IP URL.](screenshots/week-07-screenshot-25.png)

---

#### Screenshot 13 — Proof of a successful database-backed action (viewing products, adding to cart, or placing an order)

![Browser page showing a successful database-backed action in the EpicBook app, such as viewing products, adding an item to the cart, or completing an order. The main subject is the active app interface displaying a successful result, while the wider environment is a desktop browser window with a positive and successful tone. Visible text includes product, cart, or order confirmation elements.](screenshots/week-07-screenshot-26.png)

---

#### Public IP URL

Paste the public IP URL of your Virtual Machine here:

`http://20.87.114.56/`

---

# Submission Instructions

- Add all required screenshots in your submission
- Include the Virtual Machine public IP URL
- Do not expose database passwords, connection strings, or subscription IDs

---

# Completion Checklist

- [ ] Task 1: Network foundation created with public/private subnets and NSGs (Screenshots 1–3)
- [ ] Task 2: VM provisioned and required software installed (Screenshots 4–5)
- [ ] Task 3: EpicBook frontend and backend deployed (Screenshots 6–8)
- [ ] Task 4: Private Azure Database for MySQL created and data imported (Screenshots 9–11)
- [ ] Task 5: End-to-end functionality validated (Screenshots 12–13, Public IP URL)
- [ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: <https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme>  
- 🎓 University: <https://university.pravinmishra.com?utm_source=github&utm_medium=readme>  
- 💬 Discord Community: <https://discord.pravinmishra.com?utm_source=github&utm_medium=readme>  
- 📝 Blog: <https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme>  
- ▶️ YouTube Playlist: <https://www.youtube.com/playlist?list=PLFeSNDtI4Cho>  
- 🔗 Pravin Mishra (LinkedIn): <https://www.linkedin.com/in/pravin-mishra-aws-trainer/>  
- 🏢 CloudAdvisory (LinkedIn): <https://www.linkedin.com/company/thecloudadvisory/>

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
