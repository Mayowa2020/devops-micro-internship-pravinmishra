# Assignment 5 — Publish a Docker Container on Docker Hub

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will publish a Dockerized React application to Docker Hub, remove the local image tags, pull the image again from Docker Hub, and run it to verify that it can be downloaded and deployed from a container registry.

---

# Task 1 — Publish a Docker Image to Docker Hub

## Goal

Tag a locally built React image, publish it to Docker Hub, remove the local copy, pull it again from Docker Hub, and run it successfully.

### Evidence

#### Screenshot 1 — Public Docker Hub Repository

Add a screenshot of Docker Hub showing your newly created public repository:

```text
my-react-app
```

[Assignment 04 Screenshot](screenshots/week-11-screenshot-62.png)

---

#### Screenshot 2 — Successful Docker Login

Add a screenshot of the terminal showing:

```text
Login Succeeded
```

Ensure that your full name is visible and that no password, Personal Access Token, or device code is exposed.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-63.png)

---

#### Screenshot 3 — Correctly Tagged Image

Add a screenshot of the terminal showing:

```bash
docker image ls <YOUR_DOCKERHUB_USERNAME>/my-react-app
```

The output must show the `latest` tag.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-64.png)

---

#### Screenshot 4 — Successful Docker Push

Add a screenshot of the terminal showing successful completion of:

```bash
docker push <YOUR_DOCKERHUB_USERNAME>/my-react-app:latest
```

The output must include a pushed status or image digest.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-65.png)

---

#### Screenshot 5 — Published `latest` Tag in Docker Hub

Add a screenshot of your Docker Hub repository showing the uploaded `latest` image tag.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-66.png)

---

#### Screenshot 6 — Local Image Removed and Pulled Again

Add a screenshot of the terminal showing:

- The targeted local image tags removed
- Successful `docker pull` output
- `docker image ls` showing the pulled image

[Assignment 04 Screenshot](screenshots/week-11-screenshot-67.png)

---

#### Screenshot 7 — Running Pulled Image

Add a screenshot of the terminal showing:

```bash
docker ps
```

The output must show the running `react-container` with:

```text
0.0.0.0:80->80/tcp
```

[Assignment 04 Screenshot](screenshots/week-11-screenshot-68.png)

---

#### Screenshot 8 — React Application in Browser

Add a browser screenshot showing the React application at:

```text
http://20.127.27.0
```

Ensure that the VM public IP is visible in the address bar. Add your full name as a clear caption directly below the screenshot.

[Assignment 04 Screenshot](screenshots/week-11-screenshot-69.png)

---

# Docker Hub Repository URL

**Repository URL:** `https://hub.docker.com/repositories/bukky1105`

---

# Registry and Image Tagging Notes

Write a short explanation covering:

- Why image tagging is required before pushing to Docker Hub
- Why a container registry is useful in DevOps workflows
- Why production deployments should use versioned image tags instead of relying only on `latest`

## Docker Hub Image Tagging Requirements

Docker Hub relies on a specific image naming structure ([username]/[repository]:[tag]). For example, bukky1105/my-react-app:v1.0.0 to route and authenticate your uploads. If you attempt to push a locally built image without explicit tagging, the registry will reject it.
• Namespace Identification: The prefix defines your exact Docker Hub username or organization account. Without this, Docker Hub has no way of knowing which account owns the target repository.
• Registry Targeting: The tag maps your local code to a specific destination repository on the server.
• Version Control Alignment: The tag segment ensures the upload is organized under a human-readable identifier (like v1.0.0) within that repository.

## The Role of Container Registries in DevOps

A container registry acts as a centralized, secure system for storing, distributing, and managing container images across automated workflows. It is a foundational component of modern CI/CD pipelines for several key reasons:
• Single Source of Truth: It separates the build phase from the deployment phase. The CI pipeline builds and tests the image once, pushes it to the registry, and the CD pipeline pulls that exact asset to run everywhere.
• Environment Consistency: It guarantees that the identical image layer stack verified in your staging environment is the one that lands in production, eliminating "works on my machine" deployment discrepancies.
• Security & Operations Control: Modern registries provide native tools for automatic vulnerability scanning, access control lists (ACLs), audit logs, and image signing to maintain governance.

## Why Production Deployments Must Avoid latest

1. Why Production Deployments Should Use Versioned Tags Instead of latest

Relying on the implicit or explicit latest tag in production creates significant operational risks:

• Non-Deterministic Deployments: latest is simply a rolling pointer that updates every time a new image is pushed without a tag. Two servers pulling latest at slightly different times might end up running completely different builds of your application.

• Inability to Roll Back: If a deployment fails or introduces a critical bug, Kubernetes or Docker Swarm cannot automatically roll back to a previous healthy state because the image tag string (latest) hasn't changed.

• Caching Issues: Container runtimes aggressively cache images locally. If a node already has latest cached, it may fail to pull the newly pushed version, leading to inconsistent application versions across a server cluster.

• No Audit Trail: Versioned tags (such as Semantic Versioning v1.2.3 or Git commit SHAs like app:a1b2c3d) provide explicit traceability, making it easy to know exactly which source code commit is running in production.

---

# LinkedIn Requirement

## Goal

Create a LinkedIn post about publishing a Docker container image to Docker Hub.

Include:

- Assignment title: **Publish a Docker Container Image to Docker Hub**
- Your Docker Hub repository URL
- What you published
- How you verified the remote image by pulling and running it
- Key learning outcomes

### Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`(https://lnkd.in/p/eWAGpwfp)`

---

#### LinkedIn Post Screenshot

[Assignment 04 Screenshot](screenshots/week-11-screenshot-70.png)

---

# Submission Instructions

- Complete all steps in sequence.
- Include Screenshots 1–8 exactly as specified.
- Include your Docker Hub repository URL.
- Include the Registry and Image Tagging Notes.
- Include the LinkedIn post URL and screenshot.
- Ensure that your full name is visible in all terminal screenshots.
- Add your full name as a clear caption below the browser screenshot.
- Do not expose passwords, Personal Access Tokens, device codes, credentials, or other sensitive information.

---

# Completion Checklist

- [ ] Public `my-react-app` repository created
- [ ] Docker login completed successfully
- [ ] `react-multistage:latest` tagged correctly
- [ ] Image pushed to Docker Hub
- [ ] `latest` tag verified in Docker Hub
- [ ] Targeted local image tags removed
- [ ] Image pulled again from Docker Hub
- [ ] Pulled image runs successfully
- [ ] React application is accessible through the VM public IP
- [ ] Docker Hub repository URL included
- [ ] Registry and image-tagging notes completed
- [ ] LinkedIn post URL and screenshot included
- [ ] All required screenshots included
- [ ] Full name visible in terminal screenshots
- [ ] Browser screenshot has a full-name caption
- [ ] No passwords, tokens, or credentials exposed
---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
