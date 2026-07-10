# Assignment 4 — Building Your AI Team

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build and configure a set of specialized AI subagents inside your project. You will learn how different models and tool permissions define agent behavior, and you will trigger two real agent delegations to analyze security and cost aspects of your Terraform infrastructure.

---

# Task 1 — Create the Agents Folder and Add Files

## Goal

Create the `.claude/agents/` directory and add all required agent files.

### Evidence

#### Screenshot 1 — VS Code sidebar showing `.claude/agents/` with all 3 files

![Assignment 04 Screenshot](screenshots/week-02-screenshot-19.png)

---

# Task 2 — Compare the Agent Configurations

## Goal

Analyze the configuration differences between the three agents and demonstrate understanding of model and tool selection.

### Written Answers

#### 1. Why does the cost optimizer use Haiku instead of Sonnet?

The cost optimizer utilizes the Haiku model rather than Sonnet because the operations it conducts are focused on lightweight analysis tasks. These specific responsibilities include reviewing and checking Terraform resources, as well as identifying potential opportunities for cost-saving improvements.

Unlike complex operations such as security analysis, which necessitate deep reasoning capabilities, the cost optimizer's functions do not require that level of cognitive depth. Consequently, employing the faster and more cost-efficient Haiku model is highly suitable and optimal for fulfilling these specific tasks efficiently.

---

#### 2. Why does the security auditor NOT have Write in its tools list?

The Security Auditor is engineered exclusively for the inspection and analytical evaluation of Terraform configurations. In accordance with established security protocols, the deliberate omission of "Write" permissions is mandated to preclude unauthorized or inadvertent modifications to infrastructure architecture. This configuration strictly enforces the principle of least privilege, ensuring the integrity and stability of the environment remain uncompromised.

---

#### 3. Why does the tf-writer use `inherit` instead of a specific model?

The Terraform writer requires a high degree of flexibility because its core functions involve performing code generation and structural modification tasks. Utilizing the inherit setting allows the tool to dynamically adopt the default model configuration specified within Claude Code, thereby avoiding the limitations of forcing a fixed or rigid model choice for these operations.

---

### Evidence

#### Screenshot 2 — `security-auditor.md` frontmatter showing model and tools configuration

![Assignment 04 Screenshot](screenshots/week-02-screenshot-20.png)

---

#### Screenshot 3 — `cost-optimizer.md` frontmatter showing the model and tools configuration

![Assignment 04 Screenshot](screenshots/week-02-screenshot-21.png)

---

# Task 3 — Run the Security Auditor

## Goal

Trigger the security auditor agent and analyze the generated security report for your Terraform infrastructure.

### Evidence

#### Screenshot 4 — The delegation message showing Claude launched the security-auditor

![Assignment 04 Screenshot](screenshots/week-02-screenshot-22.png)

---

#### Screenshot 5 — Security audit report output

![Assignment 04 Screenshot](screenshots/week-02-screenshot-23a.png)
![Assignment 04 Screenshot](screenshots/week-02-screenshot-23b.png)
![Assignment 04 Screenshot](screenshots/week-02-screenshot-23c.png)

---

# Task 4 — Run the Cost Optimizer

## Goal

Trigger the cost optimizer agent and review the generated cost optimization report.

### Evidence

#### Screenshot 6 — The full cost optimization report

![Assignment 04 Screenshot](screenshots/week-02-screenshot-24a.png)
![Assignment 04 Screenshot](screenshots/week-02-screenshot-24b.png)
![Assignment 04 Screenshot](screenshots/week-02-screenshot-24c.png)

---

# Submission Instructions

- Ensure all agent files are committed in `.claude/agents/`
- Complete all written answers in your GitHub Repo
- Push final changes to your forked GitHub repository

---

## GitHub Repository URL

Paste your forked repository URL here:

`https://github.com/Mayowa2020/Ultimate-Agentic-DevOps-with-Claude-Code`

---

# Completion Checklist

- [✅] `.claude/agents/` folder contains all 3 agent files
- [✅] Screenshot 2 shows correct `security-auditor.md` configuration
- [✅] Screenshot 3 shows correct `cost-optimizer.md` configuration
- [✅] All 3 written answers completed
- [✅] Security auditor executed successfully
- [✅] Cost optimizer executed successfully
- [✅] Security report is visible with findings
- [✅] Cost report is visible with recommendations
- [✅ ] All required screenshots added
- [✅ ] GitHub repo updated with agents

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: <https://pravinmishra.com/dmi>  
- 🎓 DevOps for Beginners (Udemy): <https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/>  
- 🎓 Agentic AI DevOps with Claude Code: <https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/>  
- 🎓 DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm: <https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/>  
- ▶️ YouTube Playlist: <https://www.youtube.com/playlist?list=PLFeSNDtI4Cho>  
- 🔗 Pravin Mishra (LinkedIn): <https://www.linkedin.com/in/pravin-mishra-aws-trainer/>  
- 🏢 CloudAdvisory (LinkedIn): <https://www.linkedin.com/company/thecloudadvisory/>

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
