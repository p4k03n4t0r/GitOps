# GitOps

## Steps

1. Install [go](https://golang.org/doc/install)
2. Install [docker](https://docs.docker.com/engine/install/)
3. Install [kind](https://kind.sigs.k8s.io/#installation-and-usage)
4. Install [kubectl](https://kubernetes.io/docs/tasks/tools/)
5. Create kind cluster:
    ```
    kind create cluster
    ```
6. To confirm cluster is okay
    ```
    kubectl cluster-info --context kind-kind
    ```
7. Install [ArgoCD](https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd) in cluster:
    ```
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/core-install.yaml
    # to watch pods getting started:
    kubectl get -n argocd pods -w 
    ```
8. Expose ArgoCD using [port forwarding](https://argo-cd.readthedocs.io/en/stable/getting_started/#port-forwarding), since Kind doesn't have a built-in load balancer. ArgoCD will be available at https://localhost:8080.
    ```
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```
9. Initial login with username `admin` and password from the following command:
    ```
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
    ```


## Deploy manually

```
kubectl apply -f hello-world-deployment.yaml
kubectl apply -f hello-world-service.yaml
kubectl port-forward svc/hello-world 8081:5000
```