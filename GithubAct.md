# 📘 **Tutorial: Understanding the `.github/` Directory (Full Guide)**

The `.github/` folder is used to store **project automation**, **community standards**, **templates**, and **GitHub Actions**.
This folder helps you keep your repository organized, professional, and automated.

Below is an explanation of every file and directory in your structure.

---

# 📂 **`.github/` Folder Structure Overview**

```
.github/
├── CODEOWNERS
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── SECURITY.md
├── SUPPORT.md
├── FUNDING.yml
├── labels.yml
├── stale.yml
├── dependabot.yml
├── release-drafter.yml
├── ISSUE_TEMPLATE/
│   ├── bug.yml
│   ├── feature.yml
│   └── config.yml
├── PULL_REQUEST_TEMPLATE/
│   ├── feature.md
│   ├── bugfix.md
│   └── hotfix.md
├── workflows/
│   ├── ci.yml
│   ├── hotfix.yml
│   ├── staging.yml
│   ├── lint.yml
│   ├── codeql.yml
│   └── deploy.yml
└── actions/
    └── my-custom-action/
        ├── action.yml
        └── script.sh
```

---

# 🟦 **1. Community Standards Files**

These files help guide contributors and define rules for the project.

### **`CODEOWNERS`**

Defines who must review changes in specific parts of the code.

Example:

```
* @GamalMoussa
backend/** @backend-team
```

---

### **`CODE_OF_CONDUCT.md`**

Explains expected behavior in the project community.

---

### **`CONTRIBUTING.md`**

Instructions for developers on how to contribute
(e.g., branching strategy, commit rules, PR rules).

---

### **`SECURITY.md`**

How users should report security vulnerabilities.

---

### **`SUPPORT.md`**

Explains how users can get help (issues, email, docs).

---

### **`FUNDING.yml`**

Shows “Sponsor” button to support the project.

Example:

```yaml
github: tree-1917
```

---

# 🟩 **2. Automation & Management Files**

### **`labels.yml`**

Define and manage GitHub issue labels automatically.

---

### **`stale.yml`**

Auto-close old issues and PRs that are inactive.

---

### **`dependabot.yml`**

Automatically checks for dependency updates.

---

### **`release-drafter.yml`**

Automatically generates release notes
each time you merge a PR.

---

# 🟧 **3. Issue Templates**

Stored in:

```
.github/ISSUE_TEMPLATE/
```

These files create structured issue forms.

### **`bug.yml`**

Template for reporting bugs.

### **`feature.yml`**

Template for suggesting new features.

### **`config.yml`**

Controls issue behavior (e.g., disable blank issues).

Example:

```yaml
blank_issues_enabled: false
```

---

# 🟥 **4. Pull Request Templates**

Located in:

```
.github/PULL_REQUEST_TEMPLATE/
```

Used to guide contributors when creating a PR.

### Templates include:

* `feature.md`
* `bugfix.md`
* `hotfix.md`

GitHub allows selecting a template when opening a PR.

---

# 🟨 **5. GitHub Actions Workflows**

Stored in:

```
.github/workflows/
```

These YAML files define CI/CD automations.

### **`ci.yml`**

Runs tests, linters, or builds on every push/PR.

### **`hotfix.yml`**

Triggers pipeline for hotfix branches.

### **`staging.yml`**

Handles staging environment deployment.

### **`lint.yml`**

Runs code linting.

### **`codeql.yml`**

GitHub security scanning for vulnerabilities.

### **`deploy.yml`**

Production or environment deployment.

---

# 🟪 **6. Custom GitHub Actions**

Custom actions you build yourself:

```
.github/actions/my-custom-action/
```

### **`action.yml`**

Defines your custom action.

### **`script.sh`**

A shell script executed by the action.

---

# 🎉 **Conclusion**

This `.github/` structure makes your project:

✔ Professional
✔ Automated
✔ Easy to collaborate on
✔ Consistent for community contributions
✔ Ready for production workflows

This setup is ideal for DevOps, open-source projects, or team-based development.

---
