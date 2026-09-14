# Assignment 6 — Building an AI-Assisted Git Safety Net (PR Ready Check)

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In Week 2 you built Claude Code hooks that block a dangerous action *before* it happens (`PreToolUse`), and a restricted skill that could look but not touch (`allowed-tools` without `Write`). In this assignment you will discover that Git has the exact same idea, decades older: a **pre-commit hook** that blocks a commit before it's created.

You will build both halves of a real "PR Ready" workflow:

1. A **Git hook that follows fixed rules** — scans staged changes for hardcoded secrets and oversized files and refuses the commit. No AI involved, no guessing, just a rule that gives the same answer every time.
2. A **restricted Claude Code skill** (`/pr-ready`) that reads your staged diff and drafts a Pull Request title, description, and a short list of things worth a second look — the kind of judgment a fixed rule can't make (mixed changes, missing context, unclear intent). The skill never commits, pushes, or opens the PR. You do that yourself, using its draft as a starting point.

This mirrors the Agentic Loop from Week 3's Linux triage assignment: **Gather → Analyze → Human Act → Verify**. The hook and the skill both gather and analyze; only you act.

---

# Task 0 — Confirm Your Fork and Create a Feature Branch

## Goal

Confirm you are working in your own fork, then create a dedicated branch for this assignment.

### Evidence

#### Screenshot 1 — Output of git remote -v and git branch showing the new branch

![Assignment 06 Screenshot](screenshots/week-04-screenshot-51.png)

---

### Notes

**1. Why create a dedicated branch instead of doing this work on main?**

> **Creating a dedicated branch instead of working directly on the `main` branch is a Git best practice because it keeps new changes isolated from the stable codebase. This allows developers to develop, test, and refine features without affecting the production-ready version of the project. Once the feature is complete, it can be reviewed and safely merged into `main` through a Pull Request.**
>
> **From a collaboration perspective, feature branches enable multiple developers to work on different tasks simultaneously without interfering with one another's work. They also provide a clear history of changes, making it easier to track progress, review code, and revert changes if necessary.**
>
> **From a security standpoint, dedicated branches help protect the `main` branch from untested or potentially insecure code. Before changes are merged, they can undergo peer reviews, automated CI/CD checks, and security scans to identify bugs, vulnerabilities, or accidentally committed secrets such as API keys or passwords. Additionally, branch protection rules on platforms like GitHub can restrict direct pushes to `main`, requiring approved Pull Requests before changes are accepted.**
>
> **Overall, using dedicated branches promotes safer development, better collaboration, improved code quality, stronger security, and a more reliable software delivery process. This is why feature branches and Pull Requests are standard practices in professional software development and DevOps workflows.** 🚀

---

# Task 1 — Stage a Change With Realistic Risk

## Goal

On your own fork of this repository (the one you've been submitting your DMI work in since onboarding), create a new branch and stage a change that a real reviewer should catch: a hardcoded-looking secret and a leftover debug statement.

### Evidence

#### Screenshot 1 — Output of  `git status` showing the staged file on feature/ai-pr-ready

![Assignment 06 Screenshot](screenshots/week-04-screenshot-52.png)

---

### Notes

**1. Why does this assignment use an obviously fake key instead of a real one?**

This assignment uses an obviously fake AWS Access Key instead of a real one to teach secure Git practices without exposing sensitive credentials. Real API keys, passwords, and tokens should never be hardcoded into source code or committed to a Git repository because they can be permanently recorded in Git history and potentially exposed if the repository is shared or made public.

The fake key allows learners to safely practice identifying and handling secrets in a realistic scenario. It also demonstrates how easily sensitive information can end up in version control if developers are not careful. In professional DevOps environments, secrets should be stored securely using tools such as environment variables, secret management services (e.g., AWS Secrets Manager or HashiCorp Vault), or CI/CD secret stores, rather than being embedded directly in code.

Using a fake credential reinforces an important security principle: never commit real secrets to a Git repository. This helps protect cloud resources, prevents unauthorized access, and follows industry best practices for secure software development. 🔐

---

# Task 2 — Write a Real Git Pre-Commit Hook

## Goal

Create a tracked, shareable pre-commit hook that blocks a commit containing secret-like patterns or files over 1MB.

### Evidence

#### Screenshot 2 — `hooks/pre-commit` open in VS Code showing the full script

![Assignment 06 Screenshot](screenshots/week-04-screenshot-53.png)

---

#### Screenshot 3 — Output of `git config core.hooksPath` confirming it points to `hooks`

![Assignment 06 Screenshot](screenshots/week-04-screenshot-54.png)

---

### Notes

**1. Why is `hooks/pre-commit` tracked in the repo instead of living only in `.git/hooks/`?**

> **The `hooks/pre-commit` script is tracked in the repository so that it can be shared, version-controlled, and maintained consistently across the entire development team. If the hook existed only in `.git/hooks/`, it would remain a local Git configuration that is not included in the repository. This means every developer would have to create or copy the hook manually, leading to inconsistencies and increasing the risk that some team members would commit code without the required checks.**
>
> **By storing the hook in a dedicated `hooks/` directory and configuring Git with `git config core.hooksPath hooks`, the hook becomes part of the project's source code. Developers can review changes to the hook, update it through normal Git commits, and ensure everyone uses the same security and quality checks.**
>
> **From a DevOps perspective, this promotes consistency, collaboration, and security. Every developer follows the same pre-commit rules, reducing the chances of committing secrets, oversized files, or code that does not meet the project's standards. It also makes onboarding easier, as new team members can simply clone the repository, configure the hooks path, and immediately benefit from the same automated checks.**

### In summary

* ✅ **`.git/hooks/`** is **local to one developer** and is **not tracked by Git**.
* ✅ **`hooks/pre-commit`** is **tracked in the repository**, making it easy to **share, maintain, version-control, and enforce consistent security and quality checks** across the team.

This approach reflects how many professional software and DevOps teams manage Git hooks in collaborative projects. 🚀

---

**2. Compare this to `PreToolUse` from Week 2 Assignment 6. What does each one intercept, and what do they have in common?**
This question is asking you to compare **Git's `pre-commit` hook** with **Claude Code's `PreToolUse` hook**. Both are examples of **interception points**—they pause an operation before it happens so checks can be performed.

---

> **Git's `pre-commit` hook and Claude Code's `PreToolUse` hook both intercept an action before it is executed, but they operate at different stages of the development workflow.**
>
> **Git's `pre-commit` hook intercepts the `git commit` command.** It runs immediately before a commit is created, allowing checks such as detecting hardcoded secrets, preventing oversized files from being committed, enforcing coding standards, or running tests. If any check fails, the hook blocks the commit until the issues are resolved.
>
> **Claude Code's `PreToolUse` hook intercepts tool execution before Claude is allowed to use a tool.** Instead of checking source code, it examines the requested tool action—such as editing files, running shell commands, or accessing resources—and can approve, reject, or modify the request based on predefined rules or policies. This helps prevent unsafe, unintended, or unauthorized actions before they occur.
>
> **What they have in common is that both act as preventive safeguards rather than corrective measures.** They validate actions before execution, automate policy enforcement, improve security, maintain quality standards, and reduce the risk of human error. In both cases, if the checks fail, the operation is stopped until the problem is addressed.
>
> **The key difference is what they intercept:** Git's `pre-commit` hook protects the repository by checking **code and files before a commit**, while Claude Code's `PreToolUse` hook protects the execution environment by checking **tool requests before they are executed**. Both support the DevOps principle of shifting quality and security checks as early as possible in the workflow.

### Comparison Table

| Feature           | Git `pre-commit`                                                | Claude Code `PreToolUse`                    |
| ----------------- | --------------------------------------------------------------- | ------------------------------------------- |
| **Intercepts**    | `git commit`                                                    | A tool request before execution             |
| **Checks**        | Files, commits, secrets, file size, tests, code quality         | Tool usage, permissions, safety policies    |
| **Purpose**       | Prevent bad code or sensitive data from entering the repository | Prevent unsafe or unauthorized tool actions |
| **Runs**          | Before a commit is created                                      | Before Claude executes a tool               |
| **Can Block?**    | ✅ Yes                                                           | ✅ Yes                                       |
| **Primary Focus** | Code quality and repository security                            | Safe AI-assisted tool execution             |

### One-sentence answer 

> **Git's `pre-commit` hook intercepts commits before they are recorded, while Claude Code's `PreToolUse` hook intercepts tool requests before they are executed. Both act as preventive security and quality gates that enforce rules and block unsafe actions before they can occur.**

---

# Task 3 — Prove the Hook Blocks the Risky Commit

## Goal

Attempt to commit the staged file from Task 1 and show the hook rejecting it.

### Evidence

#### Screenshot 4 — Terminal showing `git commit` rejected with the hook's "BLOCKED" message naming the exact file

![Assignment 06 Screenshot](screenshots/week-04-screenshot-55.png)

---

### Notes

**1. Which line in `hooks/pre-commit` matched your fake key, and why did it match?**

For your **DMI Week 4 assignment**, here's a complete explanation:

> **The line that matched the fake AWS access key is:**
>
> ```bash
> if git diff --cached -- "$file" | grep -qE 'AKIA[0-9A-Z]{16}|-----BEGIN (RSA|OPENSSH|PRIVATE) KEY-----'; then
> ```
>
> **It matched because of the regular expression (regex):**
>
> ```text
> AKIA[0-9A-Z]{16}
> ```
>
> This pattern is designed to detect strings that look like AWS Access Key IDs. It checks for:
>
> * **`AKIA`** – the standard prefix used by many AWS Access Key IDs.
> * **`[0-9A-Z]`** – any uppercase letter (A–Z) or digit (0–9).
> * **`{16}`** – exactly 16 uppercase letters or digits following the `AKIA` prefix.
>
> The fake key used in the assignment:
>
> ```text
> AKIAABCDEFGHIJKLMNOP
> ```
>
> starts with **`AKIA`** and is followed by **16 uppercase letters (`ABCDEFGHIJKLMNOP`)**, so it perfectly matches the regular expression. As a result, the `grep` command detects it as a potential secret, sets the `blocked` flag to `1`, and prevents the commit from proceeding.
>
> This demonstrates how Git pre-commit hooks can automatically scan for credentials before code is committed, helping to prevent accidental exposure of sensitive information in a repository.

### In simple terms

The hook wasn't checking whether the key was **real**—it was checking whether it **looked like** an AWS access key. Since the fake key follows the same format as a real AWS Access Key ID, it was intentionally detected and blocked. This is how secret-scanning tools work in professional DevOps environments: they identify patterns that resemble sensitive credentials before they can be committed. 🔐

---

**2. Could this hook have caught a poorly-named variable that stores a secret without the `AKIA` prefix? What does that tell you about the limits of a fixed rule like this?**

For your **DMI Week 4 assignment**, here's a comprehensive response:

> **No, not necessarily.** This pre-commit hook is designed to detect secrets by matching specific patterns using regular expressions (regex). In this case, it looks for strings that match an AWS Access Key ID beginning with `AKIA` or private key headers such as `-----BEGIN RSA PRIVATE KEY-----`.
>
> If a developer stored a secret in a poorly named variable, such as:
>
> ```bash
> token="mySuperSecretPassword123"
> ```
>
> or
>
> ```bash
> DATABASE_PASSWORD="P@ssw0rd!"
> ```
>
> the hook would **not** detect it because these values do not match the predefined patterns in the regular expression. The hook doesn't understand the meaning or context of the variable—it only checks whether the content matches the specific rules it has been programmed to recognize.
>
> This highlights one of the main limitations of **fixed rule-based detection**: it can only identify secrets that conform to known patterns. Any secret that uses a different format, naming convention, or encoding may go undetected, resulting in **false negatives**. Conversely, it may also flag harmless strings that happen to match the pattern, resulting in **false positives**.
>
> **In professional DevOps environments, fixed-rule hooks are often combined with more advanced secret-scanning tools** such as GitHub Secret Scanning, Gitleaks, TruffleHog, or Amazon CodeGuru Reviewer. These tools use additional techniques—including entropy analysis, contextual analysis, heuristics, and large collections of known credential patterns—to detect a much wider range of secrets than simple regular expressions alone.
>
> **Therefore, while a pre-commit hook based on fixed rules is an excellent first line of defense, it should not be relied upon as the only security measure. Combining automated secret scanning, code reviews, secure secret management, and developer awareness provides much stronger protection against accidentally exposing sensitive information.** 🔐🚀

---

# Task 4 — Build the `/pr-ready` Skill

## Goal

Create a manually invoked Claude Code skill that reads your staged changes and produces a PR-readiness report and a draft PR description — without writing, committing, or pushing anything itself.

### Evidence

#### Screenshot 5 — `SKILL.md` frontmatter showing `allowed-tools: Bash, Read, Grep` (no `Write`) and `disable-model-invocation: true`

![Assignment 06 Screenshot](screenshots/week-04-screenshot-56.png)

---

#### Screenshot 6 — `/pr-ready` output while the risky file is still staged, showing it flagged the secret and/or debug statement

![Assignment 06 Screenshot](screenshots/week-04-screenshot-57.png)

---

### Notes

**1. Why does `/pr-ready` have `Bash` and `Read` but not `Write`?**

For your **DMI Week 4 assignment**, here's a comprehensive answer:

> **The `/pr-ready` command in Claude Code has access to the `Bash` and `Read` tools, but not the `Write` tool, because its primary purpose is to review and validate a project rather than modify it. This follows the principle of least privilege, which states that a process or tool should be granted only the minimum permissions necessary to perform its task.**
>
> **The `Read` tool** allows `/pr-ready` to inspect project files, source code, configuration files, and documentation so it can analyze the current state of the project.
>
> **The `Bash` tool** allows it to execute read-only or validation commands, such as:
>
> * `git status`
> * `git diff`
> * `git log`
> * running tests
> * executing linters
> * checking formatting
>
> These commands help determine whether the project is ready for a Pull Request without changing the code.
>
> **The `Write` tool is intentionally excluded** because `/pr-ready` is designed to evaluate the project, not modify it. Allowing write access could unintentionally change source files, overwrite code, or introduce new changes during the review process. By restricting write permissions, the command ensures that its assessment is based solely on the existing codebase and that developers remain in control of any changes.
>
> **This design reflects an important DevOps and security principle:** verification should be separated from modification whenever possible. Validation tools should inspect and report issues, while code changes should be made deliberately by the developer or by dedicated editing tools.
>
> **In summary, `/pr-ready` uses `Read` to inspect files and `Bash` to run validation commands, but it does not need `Write` because its role is to assess whether a project is ready for review—not to alter the code itself. This makes the command safer, more predictable, and aligned with the principle of least privilege.** 🔐🚀

---

**2. The pre-commit hook and `/pr-ready` both looked at the same staged diff. Did they flag the same things? What did one catch that the other didn't?**

For your **DMI Week 4 assignment**, here's a comprehensive response:

> **No, the pre-commit hook and `/pr-ready` did not flag exactly the same things, even though they both examined the same staged changes. They serve different purposes and perform different types of checks.**
>
> **The pre-commit hook** is a rule-based security and quality gate that runs automatically before a commit is created. It checks for specific, predefined issues, such as:
>
> * Hardcoded secrets (e.g., AWS Access Keys matching the `AKIA...` pattern)
> * Private keys
> * Oversized files (greater than 1 MB)
>
> If any of these conditions are met, the hook blocks the commit until the issue is fixed. Its checks are fast, deterministic, and based on fixed rules or regular expressions.
>
> **`/pr-ready`**, on the other hand, performs a broader review of the staged changes. Using the **Read** and **Bash** tools, it can inspect the code, run tests, check formatting, review the Git diff, and identify issues that go beyond simple pattern matching. For example, it may point out:
>
> * Missing documentation
> * Poor code structure or readability
> * Inconsistent naming conventions
> * Failing tests or linting errors
> * Files that should or should not be included in the commit
>
> Unlike the pre-commit hook, `/pr-ready` **does not modify the code** and **does not rely solely on fixed patterns**. Instead, it provides a more holistic assessment of whether the changes are suitable for a Pull Request.
>
> **One important difference is that the pre-commit hook caught the fake AWS access key because it matched the predefined `AKIA` pattern and immediately blocked the commit.** `/pr-ready` may also identify exposed secrets, but it is designed to provide a broader review of the overall quality and readiness of the changes rather than simply enforcing a few predefined rules. Conversely, `/pr-ready` could identify issues such as missing documentation, poor commit organization, or failing tests—things the pre-commit hook would not detect.
>
> **Together, they complement each other.** The pre-commit hook acts as the **first line of defense**, preventing obvious security and repository issues from being committed. `/pr-ready` serves as a **final review assistant**, evaluating the overall quality, completeness, and readiness of the changes before they are submitted for code review. Using both provides stronger protection and better code quality than relying on either one alone.

### Comparison Summary

| Pre-commit Hook                      | `/pr-ready`                                |
| ------------------------------------ | ------------------------------------------ |
| Blocks commits with known problems   | Reviews overall PR readiness               |
| Detects secrets using fixed patterns | Reviews code quality and project readiness |
| Checks file size limits              | Can run tests and linting                  |
| Fast, rule-based validation          | Broader, context-aware review              |
| Stops the commit if checks fail      | Provides feedback without modifying code   |

**In summary:** the **pre-commit hook** catches **specific, predefined security and repository issues**, while **`/pr-ready`** catches **broader code quality and readiness issues**. They inspect the same staged changes but from different perspectives, making them complementary tools in a professional DevOps workflow. 🚀

---

# Task 5 — Fix the Issues and Re-Verify

## Goal

Remove the secret and debug statement, then prove both gates now pass clean.

### Evidence

#### Screenshot 7 — `git commit` succeeding after the fix (no BLOCKED message)

![Assignment 06 Screenshot](screenshots/week-04-screenshot-58.png)

---

#### Screenshot 8 — Second `/pr-ready` run showing a clean risk report and a drafted PR title + description

![Assignment 06 Screenshot](screenshots/week-04-screenshot-59.png)

---

### Notes

**1. What exactly did you change to satisfy the pre-commit hook?**

I removed the debug echo and placeholder AWS key from scripts/notify.sh before committing. It no longer contains a hardcoded credential or prints a secret.

Why this works:

* ✅ No hardcoded credentials 
* ✅ No debug echo statements
* ✅ No credential-shaped strings
* ✅ Demonstrates best practices: set -euo pipefail, error handling, proper quoting

---

# Task 6 — Push and Open a Pull Request Using the AI Draft

## Goal

Push your branch and open a real Pull Request, using `/pr-ready`'s drafted title and description as your starting point — read it critically and edit before you use it.

**Important:** Open this Pull Request with base repository set to **your own fork** — not the shared upstream `pravinmishraaws/devops-micro-internship-pravinmishra` repository. This assignment's hook and skill files are your own practice work, not a change meant for the shared class repo.

### Evidence

#### Screenshot 9 — Your Pull Request showing the base repository is your own fork, plus the title and description, with the `/pr-ready` draft visible for comparison (paste it in the PR conversation or your notes below)

![Assignment 06 Screenshot](screenshots/week-04-screenshot-60.png)

---

#### PR Link

`(https://github.com/pravinmishraaws/devops-micro-internship-interviews/pull/430)`

---

### Notes

**1. What, if anything, did you edit in the AI's drafted PR description before using it? Why?**

I did not rewrite the PR from scratch, instead I enhanced the AI's draft with additional context and explanations by I explaining the purpose of the pre-commit hook and the Claude Code 'pr-ready' skill.

---

**2. If you had blindly copy-pasted the AI's draft without reading it, what could go wrong?**

> **If I had blindly copied and pasted the AI's draft without reviewing it, I could have submitted a Pull Request containing inaccurate, incomplete, or inappropriate information. For example, the AI draft included the line "Ready to push and create the PR with these commits" and suggested running `git push` and `gh pr create`, which may not have been appropriate for my current workflow or assignment. It also provided only a brief summary of the changes and omitted important details about the pre-commit hook and the Claude Code `pr-ready` skill.**
>
> **More generally, AI-generated content can misunderstand the context, make incorrect assumptions, or overlook important project requirements. It might also include information that is outdated or not applicable to the task. If submitted without review, this could confuse reviewers, reduce the quality of the Pull Request, or even lead to mistakes in the development workflow.**
>
> **AI should be used as an assistant, not as a replacement for human judgment. Reviewing, verifying, and refining AI-generated content ensures it is accurate, relevant, and aligned with the project's requirements and professional standards.**

---

**3. Why does this PR need to target your own fork instead of the shared upstream repository?**

> **The Pull Request (PR) should target my own fork first because I do not have direct write access to the shared upstream repository. In open-source projects, contributors typically fork the original repository into their own GitHub account, where they have full control to create branches, commit changes, and push updates safely. Once the changes are ready, they open a Pull Request from their fork back to the upstream repository for review.**
>
> **This workflow protects the upstream repository by preventing unauthorized or accidental changes to the main codebase. Project maintainers can review the proposed changes, request improvements if necessary, and decide whether to merge them. It also allows contributors to work independently without affecting other developers or the stability of the project.**
>
> **From a collaboration perspective, using a fork provides a personal workspace where developers can experiment, make multiple commits, and update their branches without impacting the shared repository. This is especially important in open-source projects with many contributors, where maintaining a stable and secure main branch is critical.**
>
> **In summary, the PR targets my own fork because it provides a safe, controlled environment for development while protecting the shared upstream repository. The fork-and-pull-request workflow enables collaboration, code review, and quality assurance before any changes are merged into the main project.**

### Simple Workflow

```text
Upstream Repository (shared)
          │
          │ Fork
          ▼
My Fork (my GitHub account)
          │
          │ Create feature branch
          ▼
feature/my-change
          │
          │ Commit & Push
          ▼
Open Pull Request
          │
          ▼
Upstream Repository (review → approve → merge)
```

> **In summary, A Pull Request targets my own fork because I have write access to my fork but not to the shared upstream repository. This protects the upstream project from unauthorized changes, allows maintainers to review contributions before merging, and provides a safe workspace for developing and testing changes independently.** 🚀

---

# Task 7 — Map the Workflow to the Agentic Loop

## Goal

This assignment follows the same **Gather → Analyze → Human Act → Verify** workflow ued in week 3**, but instead of troubleshooting a production server, it focuses on reviewing Git changes and preparing a Pull Request.

---

## Gather 🔍

The first step is to collect information about the changes before making any decisions.

In this assignment, the information is gathered by:

* Running `git status` to see which files are staged.
* Running `git diff --cached` to review the exact changes that will be committed.
* Using the pre-commit hook and Claude Code's `/pr-ready` skill to inspect the staged changes.

**Purpose:** Understand exactly what is about to be committed or submitted for review.

---

## Analyze 🧠

Once the information is collected, it is analyzed for potential issues.

The **pre-commit hook** checks for:

* Hardcoded secrets (such as AWS access keys)
* Private keys
* Oversized files

The **`/pr-ready` skill** reviews:

* Secrets or credential-like strings
* Debug statements
* TODO/FIXME comments
* Mixed or unrelated changes in one PR
* Missing documentation or notes
* Appropriate PR title and description

**Purpose:** Identify security, quality, and collaboration issues before sharing the code.

---

## Human Act 👨‍💻

After reviewing the findings, the developer decides what action to take.

Typical actions include:

* Removing hardcoded credentials.
* Deleting debug statements.
* Splitting unrelated changes into separate commits.
* Updating documentation.
* Editing and improving the AI-generated PR description.
* Staging the corrected files.
* Committing the changes.
* Opening the Pull Request when everything is ready.

**Purpose:** Human judgment is used to correct issues and ensure the changes are ready for collaboration.

---

## Verify ✅

Finally, confirm that the fixes worked.

Verification includes:

* Running `git status` to confirm the correct files are staged.
* Re-running the pre-commit hook by attempting another commit.
* Running `/pr-ready` again to ensure no significant issues remain.
* Reviewing the final PR title and description.
* Confirming that the Pull Request targets the correct repository (your fork or the upstream repository, depending on the assignment).

**Purpose:** Ensure the code meets the project's quality, security, and collaboration standards before it is shared.

---

## Workflow Summary

```text
Gather
│
├─ git status
├─ git diff --cached
├─ Pre-commit hook
└─ /pr-ready
        │
        ▼
Analyze
│
├─ Check for secrets
├─ Check for debug statements
├─ Check for TODOs
├─ Check file size
├─ Review code quality
└─ Draft PR
        │
        ▼
Human Act
│
├─ Fix issues
├─ Update code
├─ Improve PR description
├─ Stage files
└─ Commit changes
        │
        ▼
Verify
│
├─ git status
├─ Commit succeeds
├─ /pr-ready passes
└─ Open Pull Request
```


> **This assignment follows the same Gather → Analyze → Human Act → Verify workflow used in Week 3. First, I gathered information using `git status`, `git diff --cached`, the pre-commit hook, and the `/pr-ready` skill to understand the staged changes. Next, these tools analyzed the changes for security issues, debug statements, TODOs, oversized files, and overall PR readiness. Based on the findings, I acted by removing hardcoded credentials, improving the code and PR description, and preparing the changes for submission. Finally, I verified the results by ensuring the commit passed the pre-commit checks, re-running `/pr-ready`, and confirming the Pull Request was ready for review. This workflow demonstrates a structured DevOps approach where issues are identified and resolved before code is shared with others.** 🚀


### Notes

**1. Which step(s) represent Gather?**

The first step is to collect information about the changes before making any decisions.

The information is gathered by:

* Running `git status` to see which files are staged.
* Running `git diff --cached` to review the exact changes that will be committed.
* Using the pre-commit hook and Claude Code's `/pr-ready` skill to inspect the staged changes.

---

**2. Which step(s) represent Analyze?**

Once the information is collected, it is analyzed for potential issues.

The **pre-commit hook** checks for:

* Hardcoded secrets (such as AWS access keys)
* Private keys
* Oversized files

The **`/pr-ready` skill** reviews:

* Secrets or credential-like strings
* Debug statements
* TODO/FIXME comments
* Mixed or unrelated changes in one PR
* Missing documentation or notes
* Appropriate PR title and description

---

**3. Which step is Human Act, and why must a human — not Claude — run `git commit`, `git push`, and open the PR?**

> **The "Human Act" step is the point where the developer reviews the analysis, makes decisions, and performs the actual changes or approval actions.** After the information has been gathered (`git status`, `git diff --cached`) and analyzed (by the pre-commit hook and the `/pr-ready` skill), it is the human's responsibility to decide whether the code is ready to be committed, pushed, or submitted as a Pull Request.
>
> **A human—not Claude—must run `git commit`, `git push`, and open the Pull Request because these are actions that permanently affect the project's history and collaboration workflow.** A commit records changes in the repository history, a push publishes those changes to a remote repository, and a Pull Request requests that other developers review and potentially merge the changes. These actions require human judgment, accountability, and confirmation that the code is correct, complete, and ready to share.
>
> **Claude's role is to assist, not to make final decisions.** It can review staged changes, identify potential issues, suggest improvements, and draft a Pull Request description, but it cannot fully understand business requirements, team priorities, or whether the developer intentionally made certain changes. Leaving the final actions to the developer reduces the risk of accidental commits, unintended pushes, or incorrect Pull Requests.
>
> **This separation also follows the DevOps principles of human oversight and least privilege.** By restricting Claude from committing, pushing, or opening PRs, the workflow ensures that AI remains an advisory tool while the developer retains full control and responsibility for changes that affect the repository.

In summary

* **Gather:** Collect information (`git status`, `git diff --cached`).
* **Analyze:** Review changes using the pre-commit hook and `/pr-ready`.
* **Human Act:** The developer reviews the findings, fixes issues, and decides to run `git commit`, `git push`, and open the Pull Request.
* **Verify:** Confirm the commit, push, and PR are correct and ready for review.

**The Human Act step is essential because only a human can take responsibility for publishing changes. AI can provide recommendations, but committing code, pushing it to a shared repository, and requesting a merge are decisions that require human judgment, accountability, and approval.** 🚀

---

**4. Which step is Verify?**

For your **DMI Week 4 assignment**, here's a comprehensive answer:

> **The "Verify" step is the final stage of the workflow, where I confirm that the actions I took produced the expected results and that the changes are ready for collaboration.** After fixing any issues identified during the analysis and performing the required Git operations, I verify that everything is correct before considering the task complete.
>
> In this assignment, verification includes:
>
> * Confirming that the **pre-commit hook** no longer blocks the commit because the hardcoded credential has been removed.
> * Running **`git status`** to ensure the correct files are staged or that the working tree is clean after the commit.
> * Reviewing the commit history with **`git log --oneline`** to confirm the commit was created successfully.
> * Re-running **`/pr-ready`** to verify there are no remaining issues and that the generated PR title and description accurately reflect the changes.
> * Confirming that the Pull Request targets the correct repository and branch and is ready for review.
>
> **The purpose of the Verify step is to ensure that the fixes actually worked and that the code meets the project's security, quality, and collaboration standards before it is shared with others.** Verification helps catch any remaining mistakes before they reach teammates or the main codebase.


> ** To summarise, the Verify step is where I confirm that the changes are correct and ready to be shared. I verify that the pre-commit hook passes, `git status` and `git log` show the expected results, `/pr-ready` reports no significant issues, and the Pull Request is correctly prepared. This ensures the code is secure, accurate, and ready for review before it is merged.** 

---

**5. In one or two sentences: why do you need *both* the fixed-rule pre-commit hook and the AI skill? Isn't one enough?**

> **Both are needed because they serve different purposes. The fixed-rule pre-commit hook quickly and consistently blocks known issues such as hardcoded secrets and oversized files, while the AI skill provides broader, context-aware feedback on code quality, documentation, commit organization, and PR readiness. Together, they provide stronger protection and a more thorough review than either could provide on its own.** 

---

# Task 8 — LinkedIn Post

## Goal

Publish a LinkedIn post summarizing what you built and what you learned about combining fixed-rule safety checks with AI-assisted review.

### Evidence

#### LinkedIn Post URL

`https://www.linkedin.com/posts/bukky-oyetimehin_dmi-devops-git-ugcPost-7487581945858613248-B21H/?utm_source=share&utm_medium=member_desktop&rcm=ACoAABEGQlgB1AkrO3hQl21ZivPMvp3RJYKW6KI`

---

## Key Learnings
Add 3-5 bullet points on what you learned this week.

* Git Fundamentals,Branching and Version Control
* GitHub Workflow
* Open Source Collaboration
* Git Hooks $ Security
* Claude Code & AI-Assisted Development

---

# Submission Instructions

* Ensure `hooks/pre-commit` and `.claude/skills/pr-ready/SKILL.md` are committed to your GitHub repository
* Add all required screenshots to your submission
* All written answers must be in your own words
* Do not use a real secret or credential anywhere in your submission — the fake key in Task 1 is intentional and must stay clearly fake
* Open your Pull Request against your own fork, not the shared upstream repository
* Push your final changes to your forked repository
* Include your PR link and LinkedIn post URL

---

## GitHub Repository URL

Paste your forked repository URL here:

`https://github.com/Mayowa2020/devops-micro-internship-interviews)`

---

# Completion Checklist

* [ ] Branch `feature/ai-pr-ready` created with a staged file containing a fake secret and a debug statement
* [ ] `hooks/pre-commit` created and tracked in the repo (not only in `.git/hooks/`)
* [ ] `core.hooksPath` configured to point at `hooks/`
* [ ] Pre-commit hook shown blocking the risky commit
* [ ] `.claude/skills/pr-ready/SKILL.md` created with correct `allowed-tools` (no `Write`) and `disable-model-invocation: true`
* [ ] `/pr-ready` run against the risky diff and shown flagging issues
* [ ] Risky file fixed; `git commit` succeeds cleanly
* [ ] `/pr-ready` re-run showing a clean report and drafted PR title/description
* [ ] Pull Request opened using the AI draft as a starting point, with your own fork as the base repository (not upstream), PR link included
* [ ] Agentic Loop mapping (Task 7) completed in your own words
* [ ] LinkedIn post published and URL submitted
* [ ] All required screenshots added
* [ ] GitHub repository URL provided

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

* 🌐 DMI Official Website: <https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme>  
* 🎓 University: <https://university.pravinmishra.com?utm_source=github&utm_medium=readme>  
* 💬 Discord Community: <https://discord.pravinmishra.com?utm_source=github&utm_medium=readme>  
* 📝 Blog: <https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme>  
* ▶️ YouTube Playlist: <https://www.youtube.com/playlist?list=PLFeSNDtI4Cho>  
* 🔗 Pravin Mishra (LinkedIn): <https://www.linkedin.com/in/pravin-mishra-aws-trainer/>  
* 🏢 CloudAdvisory (LinkedIn): <https://www.linkedin.com/company/thecloudadvisory/>

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
