# ArgoCD GitOps Repository

This repository contains the necessary configuration to manage a Kubernetes cluster using ArgoCD. Follow the steps below to install ArgoCD and apply the configurations from this repository.

## Prerequisites

- A configured Kubernetes cluster.
- `kubectl` installed and configured to access the cluster.
- Internet access to download images and dependencies.

## Step 1: Install ArgoCD

1. **Install ArgoCD on the Kubernetes cluster:**
    ```bash
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```

2. **Wait until all pods are running:**
    ```bash
    kubectl get pods -n argocd
    ```

3. **Expose the ArgoCD service (optional):**
    To access ArgoCD via `localhost`, run:
    ```bash
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```

## Step 2: Configure the ArgoCD CLI (optional)

1. **Download the ArgoCD CLI:**
    [CLI Installation Instructions](https://argo-cd.readthedocs.io/en/stable/cli_installation/)

2. **Log in to ArgoCD:**
    ```bash
    argocd login localhost:8080
    ```
    Use the default user `admin` and retrieve the initial password:
    ```bash
    kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
    ```

## Step 3: Apply the `apps-of-apps.yaml` File

1. **Clone this repository:**
    ```bash
    git clone https://github.com/jeffersonmss/gitops.git
    cd gitops
    ```

2. **Apply the `apps-of-apps.yaml` file:**
    ```bash
    kubectl apply -f apps-of-apps.yaml
    ```

3. **Check the applications managed by ArgoCD:**
    Access the ArgoCD dashboard at `http://localhost:8080` or use the CLI:
    ```bash
    argocd app list
    ```

## Step 4: Final Configuration

After applying the `apps-of-apps.yaml` file, the cluster will be automatically configured with the applications defined in this repository.

## Contribution

Contributions are welcome! Feel free to open issues or submit pull requests.

## License

This repository is licensed under the [MIT License](LICENSE).