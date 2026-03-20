---
title: Notes
nav_order: 2
---

# 🧠 Technical Notes

## ☸️ Kubernetes & GitOps: The Enterprise Blueprint

### 🚀 Google Kubernetes Engine (GKE) Pipeline
In March 2026, I successfully engineered a full-scale CI/CD pipeline using **Cloud Build** and **GKE**, centered around a sophisticated **Decoupled GitOps Architecture**.

* **Dual-Repository Strategy**: By isolating the `app` source code from the `env` Kubernetes manifests, I established a clear **Separation of Concerns**. This prevents accidental configuration drift and ensures a robust, auditable path-to-production.
* **End-to-End Automation (IaC)**: Architected a seamless flow from `docker build` to **Artifact Registry** promotion, followed by automated manifest reconciliation via `kubectl`.
* **Security Hardening**: Implemented **Google Secret Manager** to securely inject GitHub SSH keys during runtime, achieving a "zero-cleartext" environment for all sensitive build assets.

### 🏗️ GitOps Implementation Strategy
* **Infrastructure**: Regional GKE Cluster for High Availability (HA).
* **Orchestration**: Google Cloud Build with custom `cloudbuild.yaml` logic.
* **Registry**: Google Artifact Registry with immutable image tagging.
* **CD Logic**: Leveraged dynamic manifest rendering via `sed` to automate environment-specific deployments, triggering downstream CD in the environment repository.

---

### 🛠️ SRE Post-Mortem: Real-world Troubleshooting
* **Authentication Resiliency**: Diagnosed and resolved `401 Bad Credentials` during headless initialization.  
    * **Insight**: Prioritize token-based authentication (`gh auth login --with-token`) over interactive sessions to maintain idempotent provisioning scripts.
* **Stateful Provisioning**: Navigated the latency of GKE `PROVISIONING` cycles.  
    * **Best Practice**: Implemented robust state polling and handled HTTP 409 (Conflict) errors, ensuring that the automation remains **Idempotent** and resilient to retries.

---
### ❓ SRE Deep Dive: Operational Insights

**Q1: How do you ensure only high-quality code reaches your GKE cluster?**
**A:** "I implement a **Multi-Stage CI Pipeline** where the initial step is a mandatory **Unit Testing Gate**. By utilizing specialized test environments (e.g., Python Slim), I ensure that logic errors trigger a pipeline failure before containerization begins. Furthermore, I enforce **Image Immutability** by tagging builds with the `$SHORT_SHA`, ensuring the Artifact Registry remains a reliable source of truth."

**Q2: How do you recover a failed deployment due to credential issues?**
**A:** "Once a credential issue is identified (e.g., an expired PAT), I follow a **Secret-First Remediation** strategy. I first rotate the secret in the Vault or GitHub Secrets, then either **re-trigger the deployment via a Git push** or perform a **manual job re-run**. This ensures the new deployment cycle fetches updated credentials, restoring the pipeline to a healthy state without introducing code drift."

**Q3: How do you verify a secret rotation was successful without changing code?**
**A:** "Secret rotation is an **Out-of-Band change** that doesn't trigger a new commit. To verify, I monitor the **execution logs of the next CI/CD run**. By auditing the **Deployment Stage** for successful authentication handshakes, I confirm that the new credentials from the **Secrets Vault** are correctly injected into the runtime environment without needing to modify the workflow YAML."

**Q4: How do you share technical learnings with your team?**
**A:** "I maintain a **Git-based Technical Notebook** (nube.tech). Every time I solve a complex infrastructure challenge—such as debugging OAuth handshakes or optimizing GKE provisioning—I document the **root cause and resolution**. This serves as a decentralized **Knowledge Base** that prevents redundant troubleshooting and improves our overall **MTTR** (Mean Time to Recovery)."


---

### 🔗 Related Projects
* [**Google Kubernetes Engine Pipeline (GitOps)**](projects.html#%EF%B8%8F-google-kubernetes-engine-gitops-pipeline)
* [**Nube.Tech PaaS Interactive Lab**](projects.html#%EF%B8%8F-nubetech-paas-interactive-lab)

---
*Last updated: March 20, 2026*