# AutoCI-Agent: Autonomous Self-Debugging & Self-Healing CI/CD Framework

[![IBM SkillsBuild](https://img.shields.io/badge/IBM%20SkillsBuild-Cyber%20Security%202026-blue)](https://skillsbuild.org/)
[![BharatCares](https://img.shields.io/badge/Partner-BharatCares-orange)](#)
[![Python Version](https://img.shields.io/badge/Python-3.11%2B-brightgreen)](https://python.org)
[![License](https://img.shields.io/badge/License-MIT-lightgrey)](#)

> **Official Project Repository Submission**  
> **Program:** IBM SkillsBuild Cyber Security 2026  
> **Implementing Partner:** BharatCares  
> **Student Name:** Student Name  
> **Project Name:** AutoCI-Agent  

---

## 📌 1. Project Overview

**AutoCI-Agent** is an autonomous, security-first CI/CD pipeline remediation framework designed to solve build failures, syntax regressions, and security test breaks without human intervention. Inspired by enterprise agentic engineering architectures (e.g., IBM Bob), AutoCI-Agent intercepts webhook notifications, compresses raw logs using context-hygiene subagents, evaluates candidate code patches inside isolated Docker sandboxes, and verifies zero-regression fixes before drafting pull requests.

+-----------------------------+
                 |   GitHub Webhook / Trigger  |
                 +--------------+--------------+
                                |
                                v
                 +-----------------------------+
                 |    AutoCI Webhook Engine    |
                 |         (FastAPI)           |
                 +--------------+--------------+
                                |
       +------------------------+------------------------+
       |                        |                        |
       v                        v                        v
+------------------+     +------------------+     +------------------+
|  Log Diagnostics |     | Sandbox Execution|     | Security Guard   |
|     Subagent     |     |  (Docker Container) |  |   (Bandit SAST)  |
+--------+---------+     +--------+---------+     +--------+---------+
|                        |                        |
+------------------------+------------------------+
|
v
+-----------------------------+
| Self-Healing Patch Generator|
+-----------------------------+


---

## 🚀 2. Key Features & Advantages

- **Shift-Left Automated Remediation:** Automatically diagnoses and heals failing test suites in continuous integration pipelines.
- **Context Hygiene Subagent:** Filters multi-megabyte raw build logs to extract exact stack traces, reducing LLM token costs by over 85%.
- **Ephemeral Sandbox Security:** Executes untrusted AI-generated code inside resource-limited Docker containers (512MB RAM, capped CPU), preventing host runner escapes.
- **Security Policy Guardrails:** Pre-verifies candidate patches against OWASP Top 10 and Bandit static analysis rules prior to sandbox execution.
- **Iterative Self-Healing Loop:** Multi-turn feedback state machine (up to 3 retries) guiding patch generation based on execution traces.

---

## 🛠️ 3. Technologies Used

- **Core Language:** Python 3.11+
- **API Engine:** FastAPI, Uvicorn, Pydantic
- **Containerization & Isolation:** Docker SDK, Linux Containers
- **Agent Orchestration:** LangGraph, LangChain
- **Testing & Analysis:** Pytest, Bandit, Semgrep
- **Dataset / Testbed:** Synthetic Microservice Vulnerability & Failure Dataset

---

## 📊 4. Dataset & Target Benchmark Testbed

The synthetic benchmark testbed and failure logs dataset used for training, evaluation, and sandbox validation are available at:
- **Dataset / Testbed Repository:** `https://github.com/ibm-skillsbuild-cybersecurity/autoci-agent-testbed`
- **Benchmark Contents:** 50+ standardized failing pipeline execution logs, stack traces, and vulnerable code samples (OWASP Top 10 & Pytest failures).

---

## 🔧 5. Setup & Execution Instructions

### Prerequisites
1. Python 3.11 or higher installed.
2. Docker Desktop / Engine running locally.
3. Git installed.

### Step 1: Environment Configuration
First, create your project directory and set up an isolated Python virtual environment to manage dependencies cleanly.

```bash
# Clone or setup project directory
mkdir autoci-agent && cd autoci-agent

# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### Step 2: Install Dependencies
Next, install all required project libraries specified in the requirements file.

```bash
pip install -r requirements.txt
```

### Step 3: Run the AutoCI-Agent Webhook Server
Start the FastAPI webhook orchestrator server locally to listen for incoming continuous integration failure events.

```bash
python StudentName_AutoCIAgent.py
```

### Step 4: Run CLI Demonstration Mode (Self-Test)
To test and demonstrate the self-healing workflow locally without needing an active webhook listener, run the built-in CLI test suite.

```bash
python StudentName_AutoCIAgent.py --cli-test
```

### Step 5: Simulate a Webhook Payload
You can manually test the active webhook endpoint by sending a simulated JSON payload using curl in a separate terminal window.

```bash
curl -X POST "http://localhost:8000/webhook/ci-failure" \
     -H "Content-Type: application/json" \
     -d '{
       "repo_url": "https://github.com/example/sample-python-app",
       "branch": "main",
       "commit_sha": "a1b2c3d4",
       "failing_log": "FAILED tests/test_auth.py::test_login - AssertionError: assert 401 == 200"
     }'
```
