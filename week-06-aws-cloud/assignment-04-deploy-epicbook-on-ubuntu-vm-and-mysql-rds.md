# Assignment 4 — Deploy EpicBook on Ubuntu VM + MySQL RDS with Secure Cloud Network

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will deploy the EpicBook web application in AWS using a secure two-tier architecture: an Ubuntu EC2 instance with Nginx in a public subnet, and a private MySQL RDS database with restricted security-group access. The completed deployment must prove that the frontend, backend, and private database communicate successfully end to end.

---

# Task 1 — Create VPC + Public/Private Subnets + Routing

## Goal

Create the AWS network for the EpicBook deployment with a public subnet for the EC2 instance and private subnets for Amazon RDS MySQL.

### Evidence

#### Screenshot 1 — VPC details showing CIDR 10.0.0.0/16

![AWS VPC details page for the epicbook-vpc network with the main panel showing the name and ID and the CIDR block 10.0.0.0/16 highlighted in green. The left sidebar lists VPC resources and the wider environment is the AWS Management Console with a neutral administrative tone. Text in the image includes epicbook-vpc, VPC ID, 10.0.0.0/16, and VPC dashboard.](screenshots/week-06-screenshot-10.png)

---

#### Screenshot 2 — Subnets list showing the public subnet and both private database subnets with their CIDR ranges.

![AWS VPC Subnets page with a green success banner saying You have successfully created 2 subnets. The table lists epicbook-public-subnet and epicbook-private-db-subnet-1 with status Available and CIDR ranges 10.0.1.0/24 and 10.0.2.0/24. The wider environment is the AWS console, and the tone is clear and operational. Text visible in the image includes You have successfully created 2 subnets, epicbook-private-db-subnet-1, epicbook-public-subnet, 10.0.2.0/24, and 10.0.1.0/24.](screenshots/week-06-screenshot-11.png)

---

#### Screenshot 3 — Route table showing 0.0.0.0/0 → IGW and association with the public subnet

![AWS route table details page showing the public route for 0.0.0.0/0 directed to an Internet Gateway and the association with the public subnet. The wider environment is the VPC console in AWS, and the tone is neutral and administrative. Text visible in the image includes 0.0.0.0/0, Internet Gateway, and the public subnet name.](screenshots/week-06-screenshot-12.png)

---

# Task 2 — Create Security Groups (EC2 + RDS) with Least Privilege

## Goal

Create security groups that allow users to access the EpicBook application while restricting database access to the EC2 instance.

### Evidence

#### Screenshot 4 — EC2 security-group inbound rules showing ports and sources

![AWS EC2 Security Groups page with the epicbook-ec2-sg group selected. The Inbound rules table shows HTTP on port 80 from 0.0.0.0/0 and SSH on port 22 from 102.88.168.228/32 Text visible in the image includes epicbook-ec2-sg, HTTP, port 80, SSH, port 22, and the source IP.](screenshots/week-06-screenshot-13.png)

---

#### Screenshot 5 — RDS security-group inbound rule showing MySQL 3306 allowed from the EC2 security group

![AWS RDS security group configuration page showing an inbound rule for MySQL on TCP port 3306 allowed from the epicbook-ec2-sg security group. Text visible in the image includes epicbook-rds-sg, MySQL, TCP, port 3306, and epicbook-ec2-sg.](screenshots/week-06-screenshot-14.png)

---

# Task 3 — Launch Ubuntu EC2 in Public Subnet

## Goal

Launch the EC2 instance that will host the EpicBook application and connect to it through SSH.

### Evidence

#### Screenshot 6 — EC2 instance summary showing the public IPv4 address, subnet, and security group

![AWS EC2 instance summary page for an Ubuntu server running in the public subnet. The main panel displays the instance ID, public IPv4 address, subnet name, and attached security group. The wider environment is the AWS EC2 console with a neutral administrative tone. Text visible in the image includes the public IP address, subnet, security group, and instance details.](screenshots/week-06-screenshot-15.png)

---

#### Screenshot 7 — Terminal showing a successful SSH login with the `ubuntu@...` prompt

![A terminal window showing a successful SSH login to an Ubuntu EC2 instance with the command prompt beginning with ubuntu@ and a shell ready for commands. The wider environment is a Linux command line session, and the tone is technical and successful. Text visible in the image includes ubuntu@ and the shell prompt plus command output.](screenshots/week-06-screenshot-16.png)

---

# Task 4 — Install Required Software on EC2

## Goal

Prepare Ubuntu to download and run EpicBook, receive HTTP traffic, and connect to RDS.

### Evidence

#### Screenshot 8 — Output of `node -v` and `npm -v`

![A terminal session showing Node.js and npm version output after installation. The command prompt is visible and the shell prints version numbers for both tools, confirming the runtime environment is ready. The wider environment is a Linux terminal with a neutral technical tone. Text visible in the image includes node -v and npm -v and the version numbers.](screenshots/week-06-screenshot-17.png)

---

#### Screenshot 9 — Output of `systemctl status nginx`

![A terminal command showing the status of the Nginx service with active and running output. The output confirms the web server is enabled and currently serving requests. The wider environment is a Linux systemd session with a technical, successful tone. Text visible in the image includes systemctl status nginx and the active running status.](screenshots/week-06-screenshot-18.png)

---

#### Screenshot 10 — Output of `mysql --version`

![A terminal window showing the MySQL client version output after installation. The command prompt and the mysql version string confirm the database client is installed and working. The wider environment is a Linux shell with a neutral operational tone. Text visible in the image includes mysql --version and the version number.](screenshots/week-06-screenshot-19.png)

---

# Task 5 — Create RDS MySQL in Private Subnet (No Public Access)

## Goal

Launch a private MySQL database accessible only from the application EC2 security group.

### Evidence

#### Screenshot 11 — RDS instance summary showing Publicly accessible: No

![AWS RDS instance summary page for a private MySQL database with Publicly accessible set to No. The main panel shows the DB identifier, engine, and networking details in the private VPC configuration. The wider environment is the AWS RDS console with a neutral administrative tone. Text visible in the image includes Publicly accessible, No, and the database instance details.](screenshots/week-06-screenshot-20.png)

---

#### Screenshot 12 — Connectivity & security section showing the VPC and attached security group

![AWS RDS database details page for the epicbook-db instance in the epicbook-vpc VPC. The primary subject is the Connectivity and security section, which shows the database instance, the VPC name, and the attached security group. The database is presented as a private resource with a restricted network configuration, and the section lists the security group rules used to control inbound and outbound traffic. The wider environment is the AWS RDS console for a MySQL database, with a neutral administrative tone. Visible text includes epicbook-db, epicbook-vpc, epicbook-rds-sg, and the security group rule labels for inbound and outbound traffic.](screenshots/week-06-screenshot-21.png)

---

# Task 6 — Initialize Database (SQL Dump Import)

## Goal

Download EpicBook and populate bookstore with the schema, authors, and books required by the application.

### Evidence

#### Screenshot 13 — Terminal showing successful `SHOW TABLES;` output with tables listed

![A terminal window running a MySQL client session against the RDS database. The output shows SHOW TABLES and a list of database tables, confirming the schema was created successfully. The wider environment is a Linux shell connected to a remote database, and the tone is successful and technical. Text visible in the image includes SHOW TABLES and table names.](screenshots/week-06-screenshot-22.png)

---

# Task 7 — Deploy EpicBook Backend and Configure Environment Variables

## Goal

Run EpicBook on port 8080 and connect its Sequelize models to bookstore in RDS.

### Evidence

#### Screenshot 14 — Terminal showing the repository cloned and the `ls` output

![A terminal session showing the EpicBook repository being cloned and the resulting directory listing. The shell displays repository files and confirms the project was downloaded to the EC2 instance. The wider environment is a Linux terminal with a neutral technical tone. Text visible in the image includes git clone and the ls output.](screenshots/week-06-screenshot-23.png)

---

#### Screenshot 15 — Terminal showing EpicBook running, or ss -tulpn output showing port 8080 listening

![A terminal window showing the backend service running or the socket summary with port 3000 listening. The display confirms the application is active and accessible on the EC2 instance. The wider environment is a Linux server shell with a technical, successful tone. Text visible in the image includes port 3000 and the listening status.](screenshots/week-06-screenshot-24.png)

---

#### Screenshot 16 — `curl` output proving the backend responds; a 200, 301, or 404 response is acceptable if the service responds

![A terminal command using curl to query the backend endpoint and receive an HTTP response. The output shows a valid response code such as 200, 301, or 404, proving the service is responding. The wider environment is a Linux terminal with a technical, successful tone. Text visible in the image includes curl and the HTTP status code.](screenshots/week-06-screenshot-25.png)

---

# Task 8 — Configure Nginx as a Reverse Proxy and Enable Persistence.

## Goal

Expose EpicBook through Nginx on port 80 and keep its Node.js process running after SSH disconnects and server reboots.

### Evidence

#### Screenshot 17 — `nginx -t` success output

![A terminal session showing the nginx configuration test passing successfully. The command output confirms that the Nginx configuration is valid and ready to reload. The wider environment is a Linux shell with a neutral operational tone. Text visible in the image includes nginx -t and the successful test result.](screenshots/week-06-screenshot-26.png)

---

#### Screenshot 18 — Nginx configuration snippet showing location / forwarding requests to http://127.0.0.1:8080

![An Nginx configuration file snippet showing a location block for the API path that reverse proxies requests to the backend service. The wider environment is a server configuration document in a Linux host with a technical tone. Text visible in the image includes location /api and the proxy pass target.](screenshots/week-06-screenshot-27.png)

---

# Task 9 — End-to-End Testing (Frontend ↔ Backend ↔ RDS)

## Goal

Verify the public application and its use of the imported RDS data.

### Evidence

#### Screenshot 19 — Browser showing the EpicBook application loaded with the public IP visible

![A web browser tab showing the EpicBook application loaded successfully in the browser with the public IP visible in the address bar or page context. The wider environment is a browser running on a computer connected to the public EC2 deployment. The tone is successful and user facing. Text visible in the image includes the EpicBook page and the public IP address.](screenshots/week-06-screenshot-28.png)

---

#### Screenshot 20 — Terminal showing a successful HTTP request to EpicBook through the public Nginx endpoint using curl -i http://<EC2_PUBLIC_IP>/

![A terminal session making a successful API request through the public EC2 endpoint and receiving a valid JSON or HTTP response. The wider environment is a Linux shell connected to the deployed application, and the tone is successful and technical. Text visible in the image includes curl and the API endpoint plus the response output.](screenshots/week-06-screenshot-29.png)

---

#### Screenshot 21 — Terminal showing the successful database connectivity test using `SELECT 1;` or similar

![A terminal session connecting to the private MySQL database from the EC2 instance and running a simple query such as SELECT 1. The output confirms successful connectivity to the RDS database over the private network. The wider environment is a Linux shell with a technical and successful tone. Text visible in the image includes SELECT 1 and the query result.](screenshots/week-06-screenshot-30.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not expose PEM contents, passwords, `.env` values, or other secrets

---

# Completion Checklist

- [ ] Task 1: VPC, public/private subnets, IGW, and public routing created (Screenshots 1–3)
- [ ] Task 2: Least-privilege EC2 and RDS security groups created (Screenshots 4–5)
- [ ] Task 3: Ubuntu EC2 launched in the public subnet with SSH verified (Screenshots 6–7)
- [ ] Task 4: Node.js, npm, Nginx, and MySQL client, Git installed (Screenshots 8–10)
- [ ] Task 5: Private MySQL RDS created with no public access (Screenshots 11–12)
- [ ] Task 6: Database initialized from the SQL dump (Screenshot 13)
- [ ] Task 7: Backend deployed and responding on port 8080 (Screenshots 14–16)
- [ ] Task 8: Nginx proxying requests to EpicBook, with PM2 persistence. (Screenshots 17–18)
- [ ] Task 9: Frontend, backend, and RDS verified end to end (Screenshots 19–21)
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
