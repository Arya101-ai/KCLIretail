# Kane CLI Browser Testing Pipeline (`Arya101-ai/KCLIretail`)

This repository contains the complete end-to-end implementation for automated browser testing using **Kane CLI** on **GitHub Actions**. The pipeline leverages parallel execution matrices, automated headless Chrome provisioning, and dual-layer reporting covering both **Evidence** (telemetry, action traces, network logs) and **Assurance** (diagnostic verdicts, assertion outcomes).

---

## 🎯 Goal & Objectives

* **Fully Automated Pipeline:** Execute browser testing on every commit and pull request.
* **Parallel Execution:** Scale test suites concurrently using GitHub Actions Matrix strategies.
* **Anti-Bot Resiliency:** Run tests against retail targets (`https://www.saucedemo.com`) in headless mode without anti-automation blocks.
* **Comprehensive Telemetry:** Output live **Assurance Diagnostic Verdicts** in console logs while archiving **Evidence Traces** for post-execution debugging.

---

## 🏗️ System Architecture & Technology Stack

| Component | Technology | Description |
| :--- | :--- | :--- |
| **Repository** | `Arya101-ai/KCLIretail` | Main repository hosting application code, spec files, and CI workflows. |
| **CI Platform** | GitHub Actions (`ubuntu-latest`) | Cloud runner environment executing test jobs. |
| **Runtime Environment** | Node.js 20.x | Prerequisites for installing and executing `@testmuai/kane-cli`. |
| **Browser Engine** | Headless Google Chrome | Provisioned via `browser-actions/setup-chrome@v1`. |
| **Automation Engine** | `@testmuai/kane-cli` | AI-driven browser interaction and verification engine. |
| **Secrets Management** | GitHub Repository Secrets | Manages `LT_USERNAME` and `LT_ACCESS_KEY` securely. |

---

## 💡 Key Findings & Technical Insights

1. **Anti-Automation Handling:**
   * Targeting sites protected by aggressive Cloudflare Turnstiles causes automated runs to fail.
   * Testing against retail targets like `https://www.saucedemo.com` using Kane CLI's `--headless` and `--agent` flags achieves reliable executions under 60 seconds.

2. **Evidence vs. Assurance:**
   * **Evidence:** Detailed session recordings, DOM snapshots, network traffic, and trace files saved inside `~/.testmuai/kaneai/sessions/`.
   * **Assurance:** Diagnostic verdicts, pass/fail evaluation summaries, and assertions printed directly into GitHub Actions step logs.

3. **Git Working Tree Management:**
   * Modifications to workflow files must be explicitly staged (`git add`) and committed (`git commit`) before `git push` will trigger new GitHub Actions pipeline runs.

---

## 📋 CI/CD Workflow Configuration

Below is the complete `.github/workflows/browser-tests.yml` configuration used in this pipeline:

```yaml
name: Kane CLI Integration Tests (Parallel with Assurance)

on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]
  workflow_dispatch:

jobs:
  kane-parallel-tests:
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        test-spec:
          - "Verify homepage loads and key elements render correctly"
          - "Log in with standard_user, add item to cart, and verify cart badge"
          - "Proceed to checkout, enter user details, and confirm order receipt"
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Setup Node.js Environment
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install Google Chrome
        uses: browser-actions/setup-chrome@v1

      - name: Install Kane CLI
        run: npm install -g @testmuai/kane-cli

      - name: Execute Kane Test Spec & Output Assurance Verdict
        id: kane_execution
        env:
          LT_USERNAME: ${{ secrets.LT_USERNAME }}
          LT_ACCESS_KEY: ${{ secrets.LT_ACCESS_KEY }}
        run: |
          kane-cli run "${{ matrix.test-spec }}" \
            --url "https://www.saucedemo.com" \
            --headless \
            --agent \
            --timeout 300 \
            --username "$LT_USERNAME" \
            --access-key "$LT_ACCESS_KEY"

      - name: Assurance & Evidence Summary Log
        if: always()
        run: |
          echo "====================================================="
          echo "           KANE ASSURANCE DIAGNOSTIC REPORT          "
          echo "====================================================="
          if [ -d "$HOME/.testmuai/kaneai/sessions/" ]; then
            find $HOME/.testmuai/kaneai/sessions/ -name "*.json" -o -name "*.txt" -o -name "*.log" | xargs -I {} sh -c 'echo "--- File: {} ---"; head -n 30 {}; echo ""'
          fi

      - name: Archive Evidence and Assurance Artifacts
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: kane-evidence-assurance-job-${{ strategy.job-index }}
          path: |
            ~/.testmuai/kaneai/sessions/
            .testmuai/evidence/
          retention-days: 14
```

---

## 🚀 Steps to Perform & Deploy (From Scratch)

### Step 1: Requirements Gathering & PRD Definition

Define the Product Requirements Document (PRD) for automated testing:

* **Target Web App:** SauceDemo (`https://www.saucedemo.com`).
* **Test Scenarios:** Homepage rendering, login & cart management, checkout execution.
* **Non-Functional Requirements:** Test execution time < 2 minutes, non-blocking anti-bot run, 100% telemetry capture.

### Step 2: Configure Environment & Repository Secrets

1. Navigate to your repository on GitHub: `https://github.com/Arya101-ai/KCLIretail`.
2. Go to **Settings** → **Secrets and variables** → **Actions**.
3. Click **New repository secret** and add:
   * `LT_USERNAME`: Your access username.
   * `LT_ACCESS_KEY`: Your access key.

### Step 3: Local Repository Setup

1. Open your terminal in your local project root:
   ```cmd
   cd C:\Users\himanshuarya\IdeaProjects\KCLIretail
   ```

2. Create local `.gitignore` entries to exclude local temporary execution files:
   ```cmd
   echo .testmuai/ >> .gitignore
   echo .idea/ >> .gitignore
   echo .context/ >> .gitignore
   ```

### Step 4: Create the CI Workflow File

1. Create the workflow directory structure:
   ```cmd
   mkdir -p .github/workflows
   ```

2. Create `.github/workflows/browser-tests.yml` and paste the complete YAML code provided in the **CI/CD Workflow Configuration** section above.

### Step 5: Stage, Commit, and Deploy

1. Stage all new files and documentation:
   ```cmd
   git add .github/workflows/browser-tests.yml README.md .gitignore
   ```

2. Create a deployment commit:
   ```cmd
   git commit -m "feat: complete Kane CLI parallel automation pipeline setup with docs"
   ```

3. Push to GitHub:
   ```cmd
   git push origin main
   ```

### Step 6: Verify Pipeline Execution & Download Artifacts

1. Go to `github.com/Arya101-ai/KCLIretail/actions`.
2. Select the latest active run (`feat: complete Kane CLI parallel automation...`).
3. Confirm that **3 matrix jobs** are running in parallel.
4. Expand the **`Assurance & Evidence Summary Log`** step on any job to view live diagnostic reports.
5. Scroll down to **Artifacts** on the run summary page to download the archived trace zip files (`kane-evidence-assurance-job-0`, `1`, `2`).

---

## ⚖️ Do's and Don'ts

| Do's ✓ | Don'ts ✗ |
| --- | --- |
| **DO** use matrix strategies (`strategy.matrix`) to execute test cases in parallel across independent runner VMs. | **DON'T** run test suites sequentially in a single step, which increases build times and risks timeouts. |
| **DO** attach `if: always()` to artifact upload steps so telemetry is preserved even when tests fail. | **DON'T** skip artifact archiving on test failures; diagnostic telemetry is most critical during failures. |
| **DO** manage credentials securely via GitHub Repository Secrets (`LT_USERNAME`, `LT_ACCESS_KEY`). | **DON'T** hardcode API keys or secret tokens directly inside workflow files or commit histories. |
| **DO** run `git status` to verify modified files are staged prior to pushing. | **DON'T** rely on `git push --force` without staging (`git add`) and committing (`git commit`) local edits first. |
| **DO** exclude local build artifacts (`.testmuai/`, `.idea/`) via `.gitignore`. | **DON'T** check local test run directories into remote Git branches. |
