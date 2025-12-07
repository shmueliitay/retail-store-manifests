<img src="./images/argocd-logo.png" alt="Argo CD Logo" width="80"/>


# Retail Store – ArgoCD GitOps Manifests


This repository contains the GitOps deployment manifests for the Retail Store microservices application.
It is designed to work with ArgoCD using the App-of-Apps pattern, allowing a full multi-service deployment through Git.

All Kubernetes application deployments in this repo are managed by ArgoCD and automatically synchronized to the cluster.

📁 Repository Structure
retail-store-manifests/
├── apps/
│   ├── application-cart.yaml
│   ├── application-catalog.yaml
│   ├── application-checkout.yaml
│   ├── application-orders.yaml
│   ├── application-ui.yaml
└── argocd-app.yaml

Folder & File Descriptions
File / Folder	Purpose
argocd-app.yaml	The root App-of-Apps. ArgoCD reads this file and loads all application definitions inside the apps/ folder.
apps/application-ui.yaml	Deploys the UI microservice (Helm chart).
apps/application-cart.yaml	Deploys the Cart service.
apps/application-catalog.yaml	Deploys the Catalog service.
apps/application-checkout.yaml	Deploys the Checkout service.
apps/application-orders.yaml	Deploys the Orders service.
🔗 Source Code Repositories

Each microservice is deployed directly from the GitHub source repository:

https://github.com/shmueliitay/retail-store-sample-app.git


Each Application file points to its specific Helm chart directory, for example:

src/ui/chart

src/cart/chart

src/catalog/chart

src/checkout/chart

src/orders/chart

🚀 GitOps Deployment Flow (How It Works)

Developer pushes code to the retail-store-sample-app repository.

GitHub Actions CI pipeline builds Docker images and pushes to ECR.

Chart values.yaml is updated with the new image tag.

ArgoCD detects the commit change in this repo (retail-store-manifests).

ArgoCD automatically:

syncs updated Helm charts,

deploys updated microservices to the Kubernetes cluster,

performs self-healing and pruning.

Everything is fully automated end-to-end.

🌳 App-of-Apps Pattern

This repository uses the ArgoCD App-of-Apps method:

A single root Application (argocd-app.yaml)

Loads all child applications from apps/

Each child application deploys a Helm-based microservice

This structure allows easy scaling and quick addition of new services.

✔ How to Apply the Root Application

After installing ArgoCD on your Kubernetes cluster, apply only the root manifest:

kubectl apply -f argocd-app.yaml -n argocd


ArgoCD will automatically create and sync all microservices.

🛠 Adding a New Microservice

Add a new application-<name>.yaml under apps/

Point it to its Helm chart path in the sample-app repository

Commit and push — ArgoCD will deploy it automatically

📌 Requirements

A working Kubernetes cluster

ArgoCD installed (argocd namespace)

Access to:

GitHub repository

ECR (for images)
