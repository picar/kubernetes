# ArgoCD Install

## Create Namespace

```
$ kubectl create namespace argocd
```

## Install ArgoCD

```
$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
## Check the Status of the Pods
```
 kubectl get pods -n argocd
NAME                                                READY   STATUS    RESTARTS   AGE
argocd-application-controller-0                     1/1     Running   0          43s
argocd-applicationset-controller-5b866bf4f7-nblmt   1/1     Running   0          43s
argocd-dex-server-7b6987df7-894xx                   1/1     Running   0          43s
argocd-notifications-controller-5ddc4fdfb9-4z87m    1/1     Running   0          43s
argocd-redis-ffccd77b9-84qbg                        1/1     Running   0          43s
argocd-repo-server-55bb7b784-f5h5m                  1/1     Running   0          43s
argocd-server-7c746df554-jnc4c                      1/1     Running   0          43s
```
## Set Up Port Forwarding
```
$ kubectl port-forward svc/argocd-server -n argocd 8080:443
```
## Get Initial Admin Password
```
$ kubectl get secrets -n argocd argocd-initial-admin-secret -o yaml
apiVersion: v1
data:
  password: Qkk2VkRUNFdWR21VRjhrVg==
kind: Secret
metadata:
  creationTimestamp: "2024-09-30T23:08:16Z"
  name: argocd-initial-admin-secret
  namespace: argocd
  resourceVersion: "746156"
  uid: 7c6cb99b-8ff0-4a99-bde2-e2e5eb2fa3f2
type: Opaque
$ echo Qkk2VkRUNFdWR21VRjhrVg== | base64 -d
BI6VDT4WVGmUF8kV
```
## Test a Connection with the Browser
Url: https://localhost:8080

User Name: admin
## Test a Connection Using the CLI
```
$ argocd login --insecure localhost:8080
Username: admin
Password: 
'admin:login' logged in successfully
Context 'localhost:8080' updated
```
## Create an Application from CLI
```
$ argocd app create nginx --repo https://github.com/picar//ubernetes.git --path argocd/apps/nginx --dest-server https://kubernetes.default.svc --dest-namespace default
application 'nginx' created
```
