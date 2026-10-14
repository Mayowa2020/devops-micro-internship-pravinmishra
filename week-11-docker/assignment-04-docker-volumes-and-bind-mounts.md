# Assignment 4 — Docker Volumes and Bind Mounts

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will use Docker Bind Mounts and Docker Volumes to persist logs and application data outside a container’s lifecycle. You will verify that data remains available after containers are removed and recreated.

---

# Task 1 — Persist Nginx Logs Using a Bind Mount

## Goal

Deploy an Nginx container with a Bind Mount and verify that its log files remain on the VM host after the container is removed.

### Evidence

#### Screenshot 1 — Nginx Image Pull

Add a screenshot of the terminal showing successful completion of:

```bash
docker pull nginx:alpine
```

[Assignment 04 Screenshot](screenshots/week-11-screenshot-39.png)

---

#### Screenshot 2 — Host Log Directory

Add a screenshot of the terminal showing the created host directory:

```text
$HOME/nginx-logs
```

[Assignment 04 Screenshot](screenshots/week-11-screenshot-40.png)

---

#### Screenshot 3 — Running Nginx Container with Port Mapping

Add a screenshot of the terminal showing:

```bash
docker ps
```

The output must show the `myweb` container with:

```text
0.0.0.0:80->80/tcp
```

[Assignment 04 Screenshot](screenshots/week-11-screenshot-41.png)

---

#### Screenshot 4 — Nginx Welcome Page

Add a browser screenshot showing the Nginx Welcome Page at:

```text
http://<YOUR-VM-PUBLIC-IP>
```

Ensure that the VM public IP is visible in the address bar. Add your full name as a clear caption directly below the screenshot.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-42.png)

---

#### Screenshot 5 — Bind-Mounted Log Files

Add a screenshot of the terminal showing the host log files and access-log content from:

```text
$HOME/nginx-logs
```

The output must show `access.log`, `error.log`, and an access-log entry created when you opened the Nginx page.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-43.png)

---

#### Screenshot 6 — Nginx Container Removed

Add a screenshot of the terminal showing successful completion of:

```bash
docker stop myweb
docker rm myweb
```

[Assignment 04 Screenshot](screenshots/week-11-screenshot-44.png)

---

#### Screenshot 7 — Logs Persist After Container Removal

Add a screenshot of the terminal showing that `access.log` and `error.log` still exist in:

```text
$HOME/nginx-logs
```

The access log must retain its content after the container has been removed.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-45.png)

---

# Task 2 — Share Persistent Data Using a Docker Volume

## Goal

Deploy backend and frontend containers that share data through a named Docker Volume. Verify that the data remains after both containers are removed and recreated.

### Evidence

#### Screenshot 8 — Project File Structure

Add a screenshot of the terminal showing the `two-tier-app` project structure, including separate `backend` and `frontend` directories with a `Dockerfile` and `index.js` file in each.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-46.png)

---

#### Screenshot 9 — Custom Docker Network

Add a screenshot of the terminal showing `mynetwork` in:

```bash
docker network ls
```

[Assignment 04 Screenshot](screenshots/week-11-screenshot-47.png)

---

#### Screenshot 10 — Docker Volume

Add a screenshot of the terminal showing `shared-data` in:

```bash
docker volume ls
```

[Assignment 04 Screenshot](screenshots/week-11-screenshot-48.png)

---

#### Screenshot 11 — Backend Dockerfile

Add a screenshot of the terminal showing the completed backend `Dockerfile`.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-49.png)
---

#### Screenshot 12 — Backend Image Build

Add a screenshot of the terminal showing successful completion of the `backend-app:latest` image build.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-50.png)

---

#### Screenshot 13 — Running Backend Container

Add a screenshot of the terminal showing:

```bash
docker ps
```

The output must show the running `backend` container.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-51.png)

---

#### Screenshot 14 — Frontend Dockerfile

Add a screenshot of the terminal showing the completed frontend `Dockerfile`.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-52.png)

---

#### Screenshot 15 — Frontend Image Build

Add a screenshot of the terminal showing successful completion of the `frontend-app:latest` image build.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-53.png)

---

#### Screenshot 16 — Running Backend and Frontend Containers

Add a screenshot of the terminal showing:

```bash
docker ps
```

The output must show both `backend` and `frontend` containers running. Only `frontend` must have the published port mapping:

```text
0.0.0.0:80->80/tcp
```

[Assignment 04 Screenshot](screenshots/week-11-screenshot-54.png)

---

#### Screenshot 17 — Backend Write Operation

Add a screenshot of the terminal showing a successful backend write operation to the shared Docker Volume.

The output must include:

```text
Data written: Hello from Backend!
```

[Assignment 04 Screenshot](screenshots/week-11-screenshot-55.png)

---

#### Screenshot 18 — Frontend Reads Shared Data

Add a browser screenshot showing:

```text
Hello from Backend!
```

Add your full name as a clear caption directly below the screenshot.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-56.png)

---

#### Screenshot 19 — First Shared-Data Update

Add a browser screenshot showing:

```text
Test Data 1
```

Add your full name as a clear caption directly below the screenshot.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-57.png)

---

#### Screenshot 20 — Second Shared-Data Update

Add a browser screenshot showing:

```text
Test Data 2 - New Update
```

Add your full name as a clear caption directly below the screenshot.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-58.png)

---

#### Screenshot 21 — Container Removal and Recreation

Add a screenshot of the terminal showing the `frontend` and `backend` containers removed and recreated using the same `shared-data` Docker Volume.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-59.png)

---

#### Screenshot 22 — Data Persists After Recreation

Add a browser screenshot showing:

```text
Test Data 2 - New Update
```

This proves that the `shared-data` Docker Volume outlived both application containers.

Add your full name as a clear caption directly below the screenshot.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-60.png)

---

# Storage Persistence Notes

Write a short explanation covering:

- The difference between a Bind Mount and a Docker Volume
- How Task 1 proved Bind Mount persistence
- How Task 2 proved Docker Volume persistence
- Why Docker Volumes are commonly used for application data

Core Architectural Differences

• Docker Volumes are completely managed by Docker and stored within a dedicated, isolated area of the host filesystem (/var/lib/docker/volumes/ on Linux). They are the safest and most portable way to persist data because non-Docker processes cannot easily modify them.
• Bind Mounts link a specific, user-defined absolute path on the host machine to a directory inside the container. They rely entirely on the host machine’s specific directory structure and allow any process on the host to read, write, or delete the files at any time.

Proof of Persistence from the Lab

Task 1: Bind Mount Persistence

In Task 1, I mounted a standard host folder ($HOME/nginx-logs) into the Nginx container's log directory. After generating web traffic, I explicitly ran docker rm myweb to delete the container entirely. When I inspected the host machine directory afterward, the access.log and error.log files remained intact. This proved that bind mounts keep data safely written directly to my host storage independently of the container's operational lifecycle.

Task 2: Docker Volume Persistence

In Task 2, two separate containers (backend and frontend) shared data through the Docker Volume shared-data mounted at /data.

After the backend wrote Test Data 2 - New Update into the volume and both containers were forcefully removed (docker rm -f frontend backend), recreating entirely new instances of the containers with the same volume mount restored access to the data instantly. The web browser immediately rendered Test Data 2 - New Update, proving that data stored inside a Docker Volume survives container removal and recreation.

Why Docker Volumes are Preferred for Application Data

• Decoupled Lifecycle: Volumes exist independently of container lifecycles, ensuring application state (databases, user uploads) is safe when containers are upgraded, restarted, or rescheduled.
• Multi-Container Sharing: Multiple containers can safely mount the same volume simultaneously to read and write shared state.
• Better Performance & Security: Volumes perform faster on non-Linux hosts (macOS/Windows) than bind mounts and isolate sensitive application data from unprivileged host user access.
• Lifecycle Management via CLI: Volumes can be explicitly listed, audited, inspected, and safely pruned using built-in, native commands like docker volume ls and docker volume prune.
• Cloud & Storage Integration: Docker Volumes easily integrate with external cloud storage drivers (e.g., AWS EBS, Azure Disk, NFS) without altering container application logic.

---

# Public Application URL

**Application URL:** `http://20.127.27.0`

---

# LinkedIn Requirement

## Goal

Create a LinkedIn post about Docker Volumes and Bind Mounts, including one difference between them, how you verified persistent storage, and your key learning outcomes.

### Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`(https://lnkd.in/p/e8zSumYV)`

---

#### LinkedIn Post Screenshot

Add a screenshot of the published LinkedIn post here. Include a screenshot of the application displaying shared data.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-61.png)

---

# Submission Instructions

- Complete all tasks in sequence.
- Include Screenshots 1–22 exactly as specified.
- Include the Storage Persistence Notes section.
- Include the public application URL.
- Include the LinkedIn post URL and screenshot.
- Ensure that your full name is visible in all terminal screenshots.
- Add your full name as a clear caption below every browser screenshot.
- Do not expose private keys, passwords, access keys, tokens, account IDs, or other sensitive information.

---

# Completion Checklist

- [ ] Nginx image pulled successfully
- [ ] Host log directory created
- [ ] Bind Mount configured successfully
- [ ] Nginx logs remain after container removal
- [ ] Custom Docker network created
- [ ] Docker Volume created
- [ ] Backend Dockerfile and image created
- [ ] Frontend Dockerfile and image created
- [ ] Both containers mount `shared-data`
- [ ] Backend writes data to the Docker Volume
- [ ] Frontend reads the same data from the Docker Volume
- [ ] Updated data appears after browser refresh
- [ ] Data remains after frontend and backend containers are removed and recreated
- [ ] All required screenshots included
- [ ] Storage Persistence Notes completed
- [ ] Public application URL included
- [ ] LinkedIn post URL and screenshot included
- [ ] Full name visible in terminal screenshots
- [ ] Browser screenshots have full-name captions
- [ ] No sensitive information exposed

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
