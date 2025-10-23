# Kubernetes Homelab

This repository contains the configuration for a personal homelab environment, managed using a GitOps approach with Kubernetes and ArgoCD.

## Repository Structure

The repository is structured to support an "Appset-of-Appset pattern with ArgoCD, allowing for a declarative and version-controlled setup.

-   `bootstrap/`: This directory contains the root ArgoCD ApplicationSet, which is the entry point for deploying all other applications.
-   `addons/`: This directory contains the ArgoCD ApplicationSets for various applications, categorized into `cluster-services` and `user-applications`.
-   `environments/`: This directory holds the specific Kubernetes manifests and configurations for each application, organized by environment (e.g., `default`).

## Applications

This homelab includes the following applications:

### User Applications

-   **Code Server:** A VS Code instance running in the browser.
-   **Excalidraw:** A virtual collaborative whiteboard.
-   **Homepage:** A customizable homepage for your homelab services.
-   **n8n:** A workflow automation tool.
-   **pgAdmin:** A web-based administration tool for PostgreSQL.

## Configuration

Certain applications require manual configuration before deployment. These configurations typically involve setting credentials, personal information, or other environment-specific values.

### pgAdmin

Before deploying pgAdmin, you must configure the default user email address.

-   **File:** `environments/default/addons/pgadmin/manifest.yaml`
-   **Setting:** `PGADMIN_DEFAULT_EMAIL`
-   **Action:** Replace the placeholder value `"Change Me"` with your email address.

### Cluster Services

-   **Kube Prometheus Stack:** Provides a comprehensive monitoring solution for the Kubernetes cluster using Prometheus and Grafana.
-   **Pi-hole:** A network-wide ad blocker.

## Deployment

This repository is managed by ArgoCD and follows the "app of apps" pattern. To deploy the entire homelab setup, you need a running Kubernetes cluster with ArgoCD installed.

1.  **Bootstrap the environment:**

    Apply the root ApplicationSet to your cluster. This will instruct ArgoCD to deploy all the applications defined in this repository.

    ```bash
    kubectl apply -f bootstrap/root-appset.yaml
    ```

2.  **Enable Applications:**

    Each application is controlled by a label selector in its ApplicationSet (e.g., `enable_n8n: 'true'`). To deploy an application, you need to add the corresponding label to your Kubernetes cluster in ArgoCD.

Once the root ApplicationSet is applied, ArgoCD will automatically sync the applications and deploy them to the cluster.