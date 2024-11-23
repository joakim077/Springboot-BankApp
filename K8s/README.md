# Run Bank app in EKS Cluster

### 1. EKS Cluster Configuration
- Create EKS cluster 
- nodegroup
- EBSDriver
- kubectl


### 2. Run Database

Deploy DataBase sercret and configMap
  
  ```bash    
      kubectl apply -f config.yaml
      
      kubectl apply -f secrets.yaml    
  ```
Deploy Mysql Application
  ```bash    
      
      kubectl apply -f mysql-stateful.yaml

  ```
### 3. Service for MYSQL and check connectivity

Deploy service for mysql
```bash
    kubectl apply -f mysql-svc.yaml
```

- check database by exec into mysql pod


### 4. Deploy bank application and service

```bash
    kubectl apply -f deploy-bank.yaml

    kubectl apply -f svc-bank.yaml

```

#### 5. Install and Configure the NGINX Ingress Controller
**Note** LoadBalancer is required.

Deploy the NGINX Ingress Controller:

```bash
kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml
```
Verify that the Ingress Controller pods are running:

```bash
kubectl get pods -n ingress-nginx
```
Edit the ValidatingWebhookConfiguration to ignore validation errors temporarily:

```bash
kubectl edit ValidatingWebhookConfiguration ingress-nginx-admission
```
Update the failurePolicy to Ignore:

```bash
kubectl patch validatingwebhookconfiguration ingress-nginx-admission   --type='json'   -p='[{"op": "replace", "path": "/webhooks/0/failurePolicy", "value": "Ignore"}]'
```
