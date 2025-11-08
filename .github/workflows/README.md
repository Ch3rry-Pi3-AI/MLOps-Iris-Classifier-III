# ⚙️ GitHub Actions Workflows — CI/CD Automation for MLOps Iris Classifier III

This directory contains the **GitHub Actions workflow definitions** responsible for automating the **Continuous Integration (CI)** and **Continuous Deployment (CD)** process of the **MLOps Iris Classifier III** project.

By placing workflows in `.github/workflows/`, GitHub automatically detects and executes them whenever the defined triggers (such as a push to the `main` branch) occur.

## 🧩 Overview

The main workflow file, `deploy.yml`, orchestrates the complete deployment process — from **building the Docker image** to **deploying the application on Google Kubernetes Engine (GKE)**.
It ensures that every update pushed to the `main` branch is built, containerised, tested, and deployed automatically.

## 🚀 Workflow Summary

**Workflow name:** `CI/CD Deployment to GKE`

| Stage                              | Description                                                                                                                              |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Checkout Repository**         | Retrieves the latest code from the `main` branch to prepare for building.                                                                |
| **2. Google Cloud Authentication** | Authenticates using a **Service Account JSON key** stored in GitHub Secrets (`GCP_SA_KEY`).                                              |
| **3. Cloud SDK Setup**             | Installs the latest **Google Cloud SDK**, **kubectl**, and **GKE authentication plugin** for cluster access.                             |
| **4. Docker Configuration**        | Configures Docker to use Google Artifact Registry for image storage.                                                                     |
| **5. Build and Push Image**        | Builds a container image of the Flask Iris Classifier and pushes it to the Artifact Registry with tags for both `latest` and commit SHA. |
| **6. GKE Credentials Retrieval**   | Connects to the GKE cluster using the credentials from the Service Account.                                                              |
| **7. Apply Kubernetes Manifests**  | Applies the `kubernetes-deployment.yaml` file to update or create Kubernetes resources.                                                  |
| **8. Rollout Update**              | Updates the existing deployment to use the new image and waits for rollout completion.                                                   |

## 🗝️ Required GitHub Secrets

Before this workflow can run, the following **secrets** must be configured in your GitHub repository settings under **Settings → Secrets and Variables → Actions**:

| Secret Name      | Description                                                                                                                                   |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `GCP_PROJECT_ID` | Google Cloud Project ID linked to your Kubernetes and Artifact Registry setup.                                                                |
| `GCP_SA_KEY`     | Base64-encoded JSON key for a Google Cloud **Service Account** with roles for Artifact Registry, Kubernetes Engine Admin, and Storage access. |

## 🏗️ File Structure

```
.github/
└── workflows/
    └── deploy.yml        # CI/CD pipeline for GKE deployment
```

## 💡 Deployment Flow (High-Level)

1. **Developer pushes** code to the `main` branch.
2. GitHub Actions automatically triggers the `deploy.yml` workflow.
3. The workflow:

   * Builds and pushes a new Docker image to **Artifact Registry**.
   * Connects to **Google Kubernetes Engine (GKE)**.
   * Applies the Kubernetes deployment manifest to roll out the new version.
4. The Flask-based Iris Classifier app is updated live within the Kubernetes cluster.

## 🧠 Key Technologies Used

* **GitHub Actions** – Workflow automation and CI/CD pipeline management.
* **Docker** – Containerisation of the ML application.
* **Google Artifact Registry** – Secure image storage.
* **Google Kubernetes Engine (GKE)** – Scalable cluster management for deployment.
* **kubectl & gcloud CLI** – Command-line tools for orchestration and deployment.

## ✅ Best Practices

* Use separate branches for experimentation; only merge stable changes into `main`.
* Regularly rotate your **Service Account key** in GitHub Secrets for security.
* Tag Docker images with both `latest` and commit SHA for traceability.
* Ensure `kubernetes-deployment.yaml` is correctly configured with the same image path defined in `deploy.yml`.

## 📦 Outcome

Once configured, this workflow ensures **seamless, automated deployment** from GitHub to your **GKE cluster**, guaranteeing reproducibility, scalability, and minimal manual intervention.