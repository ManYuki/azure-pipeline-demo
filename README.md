# Azure DevOps CI/CD Architecture & Go Web Service

![Go](https://img.shields.io/badge/go-%2300ADD8.svg?style=for-the-badge&logo=go&logoColor=white) ![Azure DevOps](https://img.shields.io/badge/azure%20devops-%230078D7.svg?style=for-the-badge&logo=azure-devops&logoColor=white) ![Git](https://img.shields.io/badge/git-%23F05033.svg?style=for-the-badge&logo=git&logoColor=white) ![Ubuntu](https://img.shields.io/badge/ubuntu-%23E95420.svg?style=for-the-badge&logo=ubuntu&logoColor=white)

## Overview

This repository contains a lightweight Go web service backed by a fully automated Continuous Integration (CI) pipeline engineered within Azure DevOps. Rather than focusing on complex application logic, this architecture serves as a purpose-built test target to demonstrate production-grade infrastructure, automated ephemeral testing, and complete traceability by linking source control directly to Agile project management workflows.

## System Workflow

```
[Local Ubuntu Environment]
       │ (git push)
       ▼
[Azure Repos (Git)] ──(triggers)──> [Azure Pipelines]
       │                                   │
  (links to)                         (executes on)
       │                                   ▼
[Azure Boards] <──(updates)──── [Ubuntu Cloud Runner]
 (Agile Tracking)                 * Install Go 1.22
                                  * Compile Binary
                                  * Run Smoke Test
```

## Why This Matters

- **Accelerated Delivery:** Automates the build and validation phases of the software development lifecycle, significantly reducing manual intervention and deployment bottlenecks.
- **Guaranteed Code Integrity:** Enforces strict quality gates through automated smoke tests that run instantaneously upon branch commits, preventing broken code from merging.
- **End-to-End Traceability:** Integrates code modifications and CI build results directly with Agile tracking workflows, providing engineering teams with complete operational visibility.
- **Scalable Infrastructure:** Establishes a foundational architecture capable of seamlessly routing workloads between Microsoft-hosted cloud runners and self-hosted local execution agents.

## Repository Architecture

```
.
├── main.go               # Minimalist Go web service entrypoint
├── go.mod                # Go module dependency management
├── .gitignore            # Excludes compiled binaries and IDE configurations
├── azure-pipelines.yml   # Azure DevOps CI pipeline configuration matrix
├── README.md             # System architecture and operational documentation
└── screenshots/          # Project documentation and execution visuals
```

## Step-by-Step Implementation Process

This section outlines the engineering lifecycle used to provision, build, and automate this environment from the ground up.

### Phase 1: Infrastructure and Agile Provisioning

1. **Organization and Project Initialization:** Created a dedicated Azure DevOps organization and initialized a secure, private project workspace.
2. **Process Framework Configuration:** Configured the organizational process framework from Basic to Agile to support industry-standard sprint tracking.
3. **Azure Boards Setup:** Established an active Kanban board, mapping the high-level CI/CD implementation as an Epic with granular execution tasks.

### Phase 2: Source Code Management

1. **Local Scaffolding:** Initialized a foundational Go web server on a local Ubuntu environment utilizing Go 1.22 to act as the pipeline target.
2. **Version Control Integration:** Configured local Git tracking and authenticated with Azure Repos via a scoped Personal Access Token (PAT).
3. **Network Routing Override:** Resolved local IPv6 DNS connection timeouts by explicitly forcing IPv4 transport layers during the initial upstream push to Azure.

### Phase 3: CI/CD Pipeline Automation

1. **Pipeline Configuration:** Authored a declarative `azure-pipelines.yml` configuration file mapped to the repository's `main` branch.
2. **Runner Allocation:** Provisioned and utilized Microsoft-hosted `ubuntu-latest` parallel cloud jobs to execute the build matrix.
3. **Automated Smoke Testing:** Programmed the pipeline to compile the Go binary, launch the application as a background process, execute an HTTP verification check, and safely terminate the process before reporting success.

### Phase 4: Workflow Traceability

1. **Commit Linking:** Enforced traceability by tagging git commits with specific Azure Board work item IDs, establishing a direct lineage between code changes, automated pipeline runs, and project management deliverables.

## Continuous Integration Pipeline

The automated pipeline (`azure-pipelines.yml`) runs on an `ubuntu-latest` virtual machine pool and executes the following stages on every push to the `main` branch:

1. **Toolchain Initialization:** Provisions the Go 1.22 runtime environment.
2. **Compilation:** Compiles the application source code into a statically linked executable binary.
3. **Automated Smoke Testing:** Launches the compiled application as a background process, executes an HTTP request to verify endpoint health, and safely terminates the process.

## Local Development Guide

Follow these sequential steps to configure the application environment, build the binary, and execute the service on a local Ubuntu workstation.

**Prerequisites:**
- Go 1.22 or higher installed natively
- Git CLI configured with Azure DevOps credentials

**Build and Run:**

1. Clone the repository:

```bash
git clone https://dev.azure.com/yuki-test-org/_git/azure-pipeline-demo
cd azure-pipeline-demo
```

2. Initialize dependencies:

```bash
go mod tidy
```

3. Compile the service:

```bash
go build -v -o app .
```

4. Execute the application:

```bash
./app
```

5. Validate the endpoint:

Open a secondary terminal and verify the service is successfully handling requests.

```bash
curl http://localhost:8080
```

