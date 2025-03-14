# Dependabot-Demo https://shorturl.at/OVUD2

This repository is designed to help students understand GitHub Dependabot and how to manage automated dependency updates.


---

## About This Project

This project demonstrates:
- How Dependabot automates dependency updates.
- The two types of Dependabot updates:
  - Security Updates (for fixing vulnerabilities).
  - Version Updates (for outdated dependencies).
- How to configure Dependabot for various package managers.
- How to handle pull requests created by Dependabot.

---

## Getting Started

### 1. Fork This Repository

Since this is a public demo repo, you need to fork it to your GitHub account.

To fork:
1. Click the **Fork** button (top-right corner of this page).
2. GitHub will create a copy of this repository under your account.

---

## Dependabot’s Two Main Use Cases

Dependabot provides two types of automated updates:

### 1. Dependabot Security Updates  

**Purpose:** Automatically fixes dependencies with known security vulnerabilities.  
**Triggered by:** GitHub’s security database detecting a vulnerability in your project.  

- If a dependency has a security vulnerability, Dependabot creates a pull request (PR) with the patched version.
- These updates are important for security but may introduce breaking changes in some cases.

### 2. Dependabot Version Updates  

**Purpose:** Keeps dependencies up to date, even if there are no known security vulnerabilities.  
**Triggered by:** Outdated versions detected based on the update schedule in `.github/dependabot.yml`.  

- These updates help avoid technical debt and ensure you are using the latest stable dependencies.
- By default, Dependabot checks for new minor and patch versions but can be configured to check major updates too.

### Key Differences Between Security and Version Updates  

| Feature                | Security Updates | Version Updates |
|------------------------|-----------------|----------------|
| **Purpose**           | Fix vulnerabilities | Keep dependencies up-to-date |
| **Trigger**           | GitHub security alerts | Scheduled checks (daily, weekly, etc.) |
| **Impact**           | Fixes critical issues but might introduce breaking changes | Helps avoid outdated libraries and technical debt might also break things |
| **Configuration** | Enabled via the repo settings | Also Enabled via the repo settings but requires `.github/dependabot.yml` |

For more details:  
- [Security Updates](https://docs.github.com/en/code-security/dependabot/dependabot-security-updates)  
- [Version Updates](https://docs.github.com/en/code-security/dependabot/dependabot-version-updates)  

---

### 2. Enable Dependabot in Your Fork  

Dependabot is not automatically enabled on forks. To activate it:

1. Go to your forked repository on GitHub.
2. Navigate to **Settings > Code security and analysis**.
3. Enable the following:
   - **Dependabot security updates**
   - **Dependabot version updates**

---

### 3. Configure Dependabot  

Dependabot reads from the `.github/dependabot.yml` file to determine how to update dependencies.

#### Modify the Configuration  

1. Open `.github/dependabot.yml` in your forked repository.
2. You can update settings such as:
   - **Package manager** (e.g., npm, pip, Maven)
   - **Update frequency** (daily, weekly, monthly)
   - **Target branches** (e.g., `main`, `dev`)

The full list of options can be found [here](https://docs.github.com/en/code-security/dependabot/working-with-dependabot/dependabot-options-reference)

Example snippet from `.github/dependabot.yml`:
```yaml
version: 2
updates:
  # Enable version updates for npm
  - package-ecosystem: "npm"
    # Look for `package.json` and `package-lock.json` in the root directory
    directory: "/"
    # Check the npm registry for updates every day (weekdays)
    schedule:
      interval: "daily"
      time: "08:00"
      timezone: "UTC"
    # Set a limit on how many open pull requests Dependabot can create at a time
    open-pull-requests-limit: 5
    # Allow only patch and minor updates (prevent automatic major version updates)
    versioning-strategy: "increase-if-necessary"
    # Define reviewers for Dependabot PRs
    reviewers:
      - "your-github-username"
    # Add labels to Dependabot pull requests
    labels:
      - "dependencies"
      - "dependabot"
    # Specify dependencies to ignore
    ignore:
      - dependency-name: "axios"
        update-types: ["version-update:semver-major"]
      - dependency-name: "react"
        update-types: ["version-update:semver-major", "version-update:semver-minor"]
      - dependency-name: "express"
        update-types: ["version-update:semver-major"]
      - dependency-name: "lodash"
        update-types: ["version-update:semver-major", "version-update:semver-minor"]
      - dependency-name: "mongoose"
        update-types: ["version-update:semver-major"]
      - dependency-name: "jest"
        update-types: ["version-update:semver-major"]
      - dependency-name: "typescript"
        update-types: ["version-update:semver-major", "version-update:semver-minor"]
    # Allow auto-merge for minor and patch updates
    commit-message:
      prefix: "chore(deps)"
      include: "scope"

