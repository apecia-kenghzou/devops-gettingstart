# Grafana Deployment with Ingress on Kubernetes (Windows, No Helm)

This guide outlines the steps to deploy Grafana on Kubernetes using Ingress, without Helm, specifically on a Windows environment.

---

## Prerequisites

* Kubernetes cluster running locally (e.g., with Minikube or similar)
* `kubectl` installed and configured
* Ingress-NGINX controller installed
* Windows environment
``` bash
>minikube start --driver=docker

minikube addons enable ingress

```

---

## 📁 Step 1: Navigate to Project Directory

```bash
cd C:\Users\Keng\Documents\Project\SUPERCHARGE\grafana
```

---

## 🚀 Step 2: Deploy Grafana

Apply the deployment file:

```bash
kubectl apply -f grafana-deployment.yaml
```

---

## 🌐 Step 3: Create Grafana Service

```bash
kubectl apply -f grafana-service.yaml
```

---

## 🛣️ Step 4: Apply Ingress Resource

```bash
kubectl apply -f grafana-ingress.yaml
```

If the ingress class is `<none>`, update it to use `nginx`:

```bash
kubectl apply -f grafana-ingress.yaml
```

Confirm:

```bash
kubectl get ingress
```

---

## 🔍 Step 5: Verify Services

```bash
kubectl get svc
```

Expected output should include `grafana` service with `ClusterIP` and port `80`.

---

## 📡 Step 6: Port-Forward Ingress

Enable access to ingress controller via local port:

```bash
kubectl port-forward --namespace ingress-nginx service/ingress-nginx-controller 8080:80
```


---

## 🌍 Step 8: Access Grafana via Ingress
using hosts: add the following to host file located in  C:\Windows\System32\drivers\etc\hosts

```bash
127.0.0.1 grafana.localdev.me

```

Using `curl` with `--resolve`:

```bash
curl --resolve grafana.localdev.me:8080:127.0.0.1 http://grafana.localdev.me:8080
```

Or open in your browser:

```
http://grafana.localdev.me:8080
```

---

## ✅ Verification

Ensure that:

* `grafana` pod is running
* `grafana` service is reachable
* `grafana-ingress` has class `nginx`
* Port-forwarding is active
* DNS resolution (via `--resolve` or hosts file) is working

---

## 📝 Notes

* No Helm was used
* This method uses manual `.yaml` files for deployment, service, and ingress
* The ingress controller must be installed and running

---

Enjoy your Grafana setup on Kubernetes!




### install Helm
open powershell and exec below
``` bash
choco install kubernetes-helm
```


### Execute Helm

```
helm install grafana ./helm-chart
```


### enable local Ingress


```
kubectl port-forward --namespace ingress-nginx service/ingress-nginx-controller 8080:80
```


### Test

```
via browser http://grafana.localdev.me:8080/login
or 
curl --resolve grafana.localdev.me:8080:127.0.0.1 http://grafana.localdev.me:8080
```



### how do github deploy to ur server 

you will need to have a github action

github action will need to have connection to the kube cluster 

You’ll store those kubeconfig (and any other) secrets in your Git host’s “secrets” storage—so they’re kept encrypted and only exposed to your workflows at runtime. Here’s how you do it on GitHub:
```
Go to your repository settings

Open your repo on GitHub.

Click Settings (the gear icon) in the top menu.

Navigate to “Secrets and variables” → “Actions”

In the left sidebar, find Secrets and variables, then select Actions.

This is where any Action runner–accessible secrets for this repo live.

Add a new repository secret

Click New repository secret.

Give it a name (for example, KUBE_DEV, KUBE_STAGING, or KUBE_PROD).

Paste in the base64-encoded contents of your ~/.kube/config file (or whatever secret you need).

Click Add secret.
```
(Optional) Per-environment secrets

If you’ve set up GitHub’s Environments (e.g. “dev”, “staging”, “production”), you can also click Environments in repo settings, choose your environment, and add secrets there.

Workflows that target that environment automatically get access to its secrets—and you can even require manual approvals or reviewers before they’re exposed.