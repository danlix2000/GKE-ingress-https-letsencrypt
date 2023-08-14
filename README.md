# GKE-ingress-https-letsencrypt
Setup TLS certificates using cert-manager in ingress GKE

- We will use httpd image to deploy and test https

**create namespace, deployment and service**

```
kubectl create -f namespace.yml
kubectl create -f httpd-deployment.yml
kubectl create -f  httpd-service.yml
```

**Add the official stable repo:**
```
 helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
 helm repo update
```

**Install the ingress-nginx chart:**
```
helm install my-ingress ingress-nginx/ingress-nginx
```

**Install cert-manager for Let's Encrypt SSL certificates.**

Add the Jetstack Helm repository:

```
 helm repo add jetstack https://charts.jetstack.io
 helm repo update
```

- Create ingress resource

```
kubectl create -f Ingress.yml
```


**Map ingress IP with Domain name once ingress installation finished**

- get IP address and map domain name

```
kubectl get ing,svc,po -n dev

NAME                                   CLASS   HOSTS                     ADDRESS          PORTS     AGE
ingress.networking.k8s.io/my-ingress   nginx   xxxx.xxxx.xxxx.com        x.122.154.x     80, 443    35m

```

**Set up cert-manager**

Install the cert-manager Helm chart:

```
helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.12.3 --set installCRDs=true
```

**Set up the ClusterIssuer resource for Let's Encrypt.**


```
kubectl create -f cluster-issuer.yml
```
