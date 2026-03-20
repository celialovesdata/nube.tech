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

### 🛠️ SRE Post-Mortem: Real-world Troubleshooting
* **Authentication Resiliency**: Diagnosed and resolved `401 Bad Credentials` during headless initialization. **Insight**: Prioritize token-based authentication (`gh auth login --with-token`) over interactive sessions to maintain idempotent provisioning scripts.
* **Stateful Provisioning**: Navigated the latency of GKE `PROVISIONING` cycles. **Best Practice**: Implemented robust state polling and handled HTTP 409 (Conflict) errors, ensuring that the automation remains **Idempotent** and resilient to retries.

---
*Last updated: March 2026*