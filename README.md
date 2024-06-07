# srecops
This repository contain infomation about technical topics with demo information for the srecops.

# ArgoCD installation
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

Initial passwd: t71KBPIxIQioT1e-
New Passwd: Linux4321
## Service Type Load Balancer¶
* Change the argocd-server service type to LoadBalancer:
    * kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
    * kubectl port-forward svc/argocd-server -n argocd 8080:443 &

### Access The Argo CD API Server
* By default, the Argo CD API server is not exposed with an external IP. To access the API server, choose one of the following techniques to expose the Argo CD API server:

#### Service Type Load Balancer
* Change the argocd-server service type to LoadBalancer:
* kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
* argocd admin initial-password -n argocd
*  argocd login lvmql700
* kubectl port-forward svc/argocd-server -n argocd 8080:443
* kubectl port-forward svc/argocd-server -n argocd 8080:443
* argocd login 127.0.0.1
* argocd login 
* argocd login https://localhost:8080

## Login argo cd 
* argocd login localhost:8080

## Change the password using the command:
* argocd account update-password

## Register A Cluster To Deploy Apps 

