##  **Coding Styles and Conventions**

This document outlines the coding styles, conventions, and architectural principles to be used by LLM and coding agents for all projects. The goal is to establish a consistent and efficient development process.

---

## **1\. Agent Workflow & Onboarding**

**This is the first section the agent must read and follow upon starting or resuming work.**

### **1.1. For a New Project**

* Follow the **Planning Mode** detailed in Section 2.1 to establish the project's foundation.

### **1.2. For Existing Projects New to this Workflow**

If you detect that this is the first time you are working on this project (i.e., files like this `CODING_CONVENTIONS.md` or a `TODO.md` are missing), you must ask the human operator how to proceed:

**"I've noticed this project doesn't follow the standard agent-driven workflow. How should we proceed?"**

* **"Option A: Backfill & Integrate."** We will create all the standard documentation (`CODING_CONVENTIONS.md`, `TODO.md`, `docs/adr/`, etc.) and automation scripts. These will be added to the project and committed to the Git history, formally adopting the new process.  
* **"Option B: Internal Tracking Only."** The existing project structure will be left as-is. I will create a git-ignored folder (`.agent_workspace/`) to manage my progress and context internally, without modifying the project's public-facing structure.

You must wait for a clear decision before proceeding with any coding tasks.

### **1.3. When Resuming Work**

When you begin a work session on an existing project, you **must** follow these steps to re-establish context:

1. **Read the `README.md`**: To refresh your understanding of the project's overall objective and structure.  
2. **Review the Project Task List**: To understand the current progress and identify the next high-level task. Look for a `TODO.md` file first. If it doesn't exist, look for a "TODO", "Tasks", or similar section in other project documents.  
   * **Clarification**: This refers to a high-level project management list, **not** low-level `// TODO:` or `# TODO:` comments found within source code files.  
3. **Consult `docs/adr/`**: Review the most recent Architectural Decision Records to understand the "why" behind significant technical choices.  
4. **Reference This Document (`CODING_CONVENTIONS.md`)**: Ensure your subsequent work adheres to these established rules.

### **1.4. Task Management (Using the Project Task List)**

The project's task list (`TODO.md` or its equivalent) is your primary tool for tracking progress.

* **Before Starting a Task**: Modify the task list. Mark the task you are about to work on as `[WIP]` (Work In Progress).  
* **After Completing a Task**: Update the task list by changing `[WIP]` to `[x]`.

---

## **2\. Planning & Project Initialization**

### **2.1. Planning Mode**

Every new project must begin in a **planning mode**.

* **Native Planning Mode**: If the coding agent has a native planning feature, it must be used. This native mode **must be augmented** by the conversational approach of the manual mode. The agent should still engage in a discussion to cover all points outlined below before producing its final one-pager plan.  
* **Manual Planning Mode**: If no native planning mode exists, the agent must engage in a detailed conversation to:  
  * **Understand Requirements**: Ask clarifying questions to fully grasp the project goals.  
  * **Define Scope**: Create and refine a specification or task list based on the human's input.  
  * **Architectural Discussion**: Discuss and decide on key architectural aspects, including user experience, cloud resources, cost, security, and scalability.

### **2.2. Planning Artifacts**

The planning phase, whether native or manual, must produce the following artifacts. These documents are also expected to be generated when onboarding an existing project that lacks them.

1.  **PRFAQ Document**: A document inspired by Amazon's internal process for new product development, to be located at `docs/PRFAQ.md`. It consists of:
    *   **Press Release (PR)**: A one-page narrative describing the product/feature from the customer's perspective. It announces the finished product, highlighting the customer problem and how the new functionality solves it.
    *   **Frequently Asked Questions (FAQ)**: A list of anticipated questions from both customers and internal stakeholders, along with clear, concise answers. This helps to think through potential challenges, dependencies, and edge cases.

2.  **One-Page Tech Spec**: A concise technical document that outlines the proposed solution, to be located at `docs/TECH_SPEC.md`. It should include:
    *   **System Actors**: A list of all users or systems that will interact with the new application.
    *   **User Stories & Flows**: A description of the key user journeys and workflows. This can be a list of user stories (e.g., "As a [user type], I want to [action] so that [benefit]") and diagrams or descriptions of the flows.
    *   High-level architecture diagram.
    *   Data models.
    *   API endpoints (if applicable).
    *   **Target Output Schematic**: For projects that generate a structured text or UI output (e.g., a report or a web page), the `TECH_SPEC.md` should include a "Target Output Schematic". This should be a literal, formatted block (e.g., a Markdown blockquote) that shows the exact desired structure of the output, using placeholders for dynamic content. This is more explicit than user stories alone.
    *   Key technical decisions and trade-offs.

### **2.3. Project Scaffolding**

Upon initialization, the agent must create the following structure. The `.gitignore` file should always include the agent's workspace directory in case it is used.

**Note:** The `.agent_workspace/` directory should only be created if **Option B** from Section 1.2 is chosen. For all standard new projects, this directory should be omitted.

\# .gitignore example  
.agent\_workspace/

.  
├── .agent\_workspace/   \# Optional: For internal agent tracking only. See 1.2.  
├── .github/  
│   └── workflows/      \# CI/CD pipelines  
├── docs/  
│   ├── adr/            \# Architectural Decision Records
│   ├── PRFAQ.md
│   └── TECH_SPEC.md
├── iac/                \# Infrastructure as Code (OpenTofu)  
├── scripts/            \# Build, deploy, test, lint scripts  
├── src/                \# Source code  
├── .gitignore  
├── CODING\_CONVENTIONS.md \# This document  
├── Dockerfile  
├── README.md  
└── TODO.md             \# Or other manifest file

### **2.4. Technology Stack Validation**
*   Before any code is written, the agent's plan must include a **"Technology Validation"** step. This step requires the agent to explicitly cross-reference all functional and non-functional requirements (e.g., use of a specific library like `dspy` from Section 6) against the chosen technology stack (from Section 5). The agent must state the final, validated language and key libraries for the project. This must be approved by the human operator before proceeding.

---

## **3\. Documentation**

### **3.1. README.md**

The `README.md` is for an external reader. It must clearly and concisely explain:

* **Project Objective**: What problem does this project solve? What is its context?  
* **Project Type**: Is it a web service, a CLI tool, a library, etc.?  
* **How to Build & Run**: Provide step-by-step instructions for building, running, and testing.  
* **Deployment**: Clear instructions for deploying to QA and Production environments.

### **3.2. Architectural Decision Records (ADRs)**

Significant architectural decisions must be documented as ADRs.

* **Location**: `docs/adr/`  
* **Tooling**: Use `adr-tools`.  
* The agent is responsible for **proposing and updating ADRs** as the project evolves.

---

##  **4\. Architectural & Development Principles**

### **4.1. Core Principles**

The agent must adhere to the following principles whenever applicable:

* **KISS (Keep It Simple, Stupid)**: Prefer the simplest solution that effectively solves the problem.  
* **SOLID**:  
  * **S \- Single Responsibility Principle**: A class or module should have one, and only one, reason to change.  
  * **O \- Open/Closed Principle**: Software entities (classes, modules, functions) should be open for extension but closed for modification.  
  * **L \- Liskov Substitution Principle**: Subtypes must be substitutable for their base types without altering the correctness of the program.  
  * **I \- Interface Segregation Principle**: No client should be forced to depend on methods it does not use. Prefer many small, specific interfaces over one large, general-purpose one.  
  * **D \- Dependency Inversion Principle**: High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions.  
* **Command Query Responsibility Segregation (CQRS)**: Separates the models for reading (Query) and writing (Command) data. This principle extends beyond databases; **functions/methods that only gather data (Queries) should be kept separate from those that have side effects (Commands)**. This optimizes performance, scalability, and clarity.

### **4.2. Architectural Considerations**

A **Client-Server architecture** with a separate frontend and backend is the default for all applicable projects. The primary architectural decision for the backend is choosing between a **Monolithic** or **Microservices** approach.

The following patterns should be considered during the planning phase as strong suggestions.

* **Primary Backend Consideration**:  
  * **Monolithic Architecture**: A single, unified codebase and application. Simpler to develop, test, and deploy initially. A good starting point for MVPs and smaller applications.  
  * **Microservices Architecture**: Structures an-application as a collection of small, independently deployable services. Excellent for complex systems requiring high scalability and team autonomy, but introduces operational overhead.  
* **Other Relevant Patterns**:  
  * **Event-Driven Architecture (EDA)**: Components communicate asynchronously via events. This pattern enables highly scalable, decoupled systems that are resilient to failures. Ideal for applications that need to react to real-time changes.  
  * **Layered (N-tier) Architecture**: Separates components into horizontal layers (e.g., Presentation, Business, Persistence, Database). It's a traditional, well-understood pattern good for many applications.  
* **Secondary Patterns (Lower Weight)**: These patterns are valuable but should be considered with less weight than the primary options above.  
  * **Model-View-Controller (MVC)**: A pattern common in web frameworks that can inform component structure.  
  * **Hexagonal Architecture (Ports and Adapters)**: An advanced pattern to consider for complex systems requiring high testability and maintainability.

### **4.3. Human Verification**

The agent must **proactively verify** with the human operator on critical aspects like:

* Scalability factors and potential bottlenecks.  
* Security vulnerabilities and best practices.  
* Significant architectural changes.

---

## **5\. Technology Stack & Languages**

* **Backend, CLI, Scripting**: **Go**, **Python**, **Ruby**, and **Shell**. The agent should propose the best language based on the task's complexity, the availability of native libraries, and deployment requirements.  
  * **Python Web Framework**: Default to **FastAPI**.  
  * **Ruby Web Framework**: Default to **Ruby on Rails**.  
* **Frontend**: **JavaScript** with the **React** framework. The agent is expected to handle all aspects of frontend development, including HTML and CSS.  
* **Containerization**: **Docker** is the standard for containerizing applications. A `Dockerfile` must be included.

### **5.1. Project Setup & Environment Management**
*   **Initialization**: For any chosen language, the first step after scaffolding is to initialize the project using its standard dependency management system. This ensures the project is self-contained and reproducible.
    *   **Python**: Create a virtual environment (e.g., `python3 -m venv venv`) and activate it. All subsequent `pip` and `python` commands must be run from within this environment.
    *   **Go**: Initialize a module (e.g., `go mod init <module-path>`) and manage dependencies with `go mod tidy`.
    *   **JavaScript/Node.js**: Initialize a project with `npm init -y` and manage packages with `npm install`.
*   **Ignoring Artifacts**: Language-specific environment and dependency directories (e.g., `venv/`, `node_modules/`) and build artifacts must be added to the `.gitignore` file.
*   **Documentation & Automation**: The specific setup and installation steps must be:
    *   Clearly documented in the `README.md` under a "Getting Started" or "Build & Run" section.
    *   Automated via a setup script in the `scripts/` directory (e.g., `scripts/setup.sh`).

---

## **6\. Programmatic LLM Interaction**

* For any feature that requires programmatically constructing prompts to interact with Large Language Models (LLMs), the project must use the **`dspy` library** (`https://github.com/stanfordnlp/dspy`). This ensures a structured, modular, and optimizable approach to prompting.

---

## **7\. Infrastructure as Code (IaC)**

All projects requiring cloud infrastructure deployment must define that infrastructure as code.

* **Default Tool**: **OpenTofu** is the default IaC tool.  
* **Location**: All OpenTofu code must be located in the `iac/` directory at the project root.  
* **Remote State**: The IaC code should include the necessary configuration to set up a remote backend (e.g., on S3, Azure Blob Storage, etc.) once per project. The configurations for the **QA and Production environments must be configured to use this remote backend** for shared and consistent state management.

---

## **8\. Metrics & Monitoring**

All applications must include basic capabilities for logging and emitting metrics to ensure observability.

* **Logging**: A standard, reputable logger library for the chosen language must be used.  
* **Metrics**: The application should emit basic metrics (e.g., request counts, error rates, processing duration) to provide insight into its operational state.  
* **Security**: **Under no circumstances should sensitive information be logged.** This includes, but is not limited to, API keys, database credentials, passwords, and personal user data. The agent must implement safeguards to prevent this.  
* **CLI Tools**:  
  * Logging should be directed to a file within a temporary local folder (e.g., `/tmp/project-name/`).  
  * A `--verbose` or `-v` flag must be implemented to allow for more detailed debugging output on the console when needed.

---

## **9\. Automation & Scripts**

The `scripts/` directory will contain **Bash scripts** for all common development and operational tasks. These scripts should be executable and serve as the single point of entry for automation.

* `./scripts/setup.sh`: To set up the development environment and install dependencies.
* `./scripts/lint.sh`: To run code linters.  
* `./scripts/build.sh`: To compile or build the application.  
* `./scripts/test-local.sh`: To run all tests locally.  
* `./scripts/test-qa.sh`: To run tests against the QA environment.  
* `./scripts/deploy-qa.sh`: To deploy the application to the QA environment.  
* `./scripts/deploy-prod.sh`: To deploy the application to the Production environment.  
* `./scripts/pause-infra.sh`: If using long-running cloud resources (e.g., Virtual Machines, Kubernetes Engine clusters), this script should be created to pause or scale them down to minimize costs during idle periods.  
* `./scripts/resume-infra.sh`: Complements the pause script to resume normal operations.  
  * *Note: These cost-control scripts can be ignored for resources that are serverless, on-demand, or have negligible long-term storage costs, such as Cloud Run, Artifact Registry, or S3 buckets.*

---

## **10\. Version Control**

* **Branching Strategy**: Use **GitHub Flow**.  
  1. Create a branch from `main`.  
  2. Add commits.  
  3. Open a Pull Request.  
  4. Review and merge into `main`.  
  5. Deploy `main`.  
* **Commit Messages**: Use **Conventional Commits**. While not strictly enforced by tooling, the agent should follow this convention. Tools like `commitizen/cz-cli` are recommended. Semantic Versioning should be used for tagging and releases.

---

## **11\. End-of-Session Reflection**

To foster continuous improvement in our collaborative workflow, the human operator may trigger a reflection protocol at the end of a work session.

*   **Trigger**: The human can initiate this with a prompt like, "Let's reflect on our session," or a similar request.
*   **Agent's Task**: Upon receiving the trigger, the agent should provide a structured reflection. A default starting point for this reflection is:
    > "Reflecting on our interactions and the project's progress, what can be added to the `CODING_CONVENTIONS.md` to improve our workflow next time? Based on your reflection, please also comment on the effectiveness of the coding conventions and my performance as a human guide throughout this session."
*   **Goal**: The objective is to identify friction points, refine rules, and make the agent-human collaboration more efficient and effective over time. The output of this reflection can be used to propose concrete changes to this document.

