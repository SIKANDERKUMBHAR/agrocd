
# Terminal Command History for K8s Kind Voting App

## 1. Creating and Managing Kubernetes Cluster with Kind

- Clear terminal:
  ```bash
  clear
  ```

- Create a 3-node Kubernetes cluster using Kind:
  ```bash
  kind create cluster --config=config.yml
  ```

- Check cluster information:
  ```bash
  kubectl cluster-info --context kind-kind
  kubectl get nodes
  kind get clusters
  ```

---

## 2. Installing kubectl

- Download `kubectl` for managing Kubernetes clusters:
  ```bash
  curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
  chmod +x ./kubectl
  sudo mv ./kubectl /usr/local/bin
  kubectl version --short --client
  ```

---

## 3. Managing Docker and Kubernetes Pods

- Check Docker containers running:
  ```bash
  docker ps
  ```

- List all Kubernetes pods in all namespaces:
  ```bash
  kubectl get pods -A
  ```

---

## 4. Cloning and Running the Example Voting App

- Clone the voting app repository:
  ```bash
  git clone https://github.com/dockersamples/example-voting-app.git
  cd example-voting-app/
  ```

- Apply Kubernetes YAML specifications for the voting app:
  ```bash
  kubectl apply -f k8s-specifications/
  ```

- List all Kubernetes resources:
  ```bash
  kubectl get all
  ```

- Forward local ports for accessing the voting and result apps:
  ```bash
  kubectl port-forward service/vote 5000:5000 --address=0.0.0.0 &
  kubectl port-forward service/result 5001:5001 --address=0.0.0.0 &
  ```

---

## 5. Managing Files in Example Voting App

- Navigate and view files:
  ```bash
  cd ..
  cd seed-data/
  ls
  cat Dockerfile
  cat generate-votes.sh
  ```

---

## 6. Installing Argo CD

- Create a namespace for Argo CD:
  ```bash
  kubectl create namespace argocd
  ```

- Apply the Argo CD manifest:
  ```bash
  kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
  ```

- Check services in Argo CD namespace:
  ```bash
  kubectl get svc -n argocd
  ```

- Expose Argo CD server using NodePort:
  ```bash
  kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
  ```

- Forward ports to access Argo CD server:
  ```bash
  kubectl port-forward -n argocd service/argocd-server 8443:443 &
  ```

---

## 7. Deleting Kubernetes Cluster

- Delete the Kind cluster:
  ```bash
  kind delete cluster --name=kind
  ```

---

## 8. Installing Kubernetes Dashboard

- Deploy Kubernetes dashboard:
  ```bash
  kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
  ```

- Create a token for dashboard access:
  ```bash
  kubectl -n kubernetes-dashboard create token admin-user
  ```

---

## 9. Argo CD Initial Admin Password

- Retrieve Argo CD admin password:
  ```bash
  kubectl get secret -n argocd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
  ```

