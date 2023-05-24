https://codersociety.com/blog/articles/helm-best-practices

https://github.com/kairen/kubeadm-ansible

https://kubernetes.io/docs/setup/production-environment/
https://kubernetes.io/docs/setup/best-practices/
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/
https://kubernetes.io/docs/concepts/configuration/secret/
https://developer.hashicorp.com/vault/docs/platform/k8s/helm/run#architecture
https://kubernetes.io/docs/tasks/administer-cluster/securing-a-cluster/
https://developer.hashicorp.com/vault/docs/platform/k8s/helm/configuration
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
https://github.com/techiescamp/vagrant-kubeadm-kubernetes

sudo apt update && sudo apt upgrade -y && sudo reboot

ssh root@161.35.116.181 -i ~/.ssh/id_rsa

ansible-playbook -i hosts k8s-kubeadm-play-book.yaml

kubectl create deployment nginx --image nginx
kubectl expose deployment nginx --type NodePort --port 80
kubectl expose deployment nginx --type LoadBalancer --port 80
kubectl get svc
kubectl scale --replicas=3 deployment.apps/nginx

scp root@161.35.116.181:~/.kube/config ~/.kube/config

http://143.198.68.179:32186/

***

PIPELINE CI/CD Multicloud

1) Terraform: Provisionamento.

2) Kubernetes.
   - Cluster (alta disponibilidade)
   - ContainerRegistry
   - Security/Volumes/Taint/Limits/Scale
   - IngressController/Traefik

3) Observabilidade
   - Prometheus
   - Grafana   

4) Serviços:
   - https://docs.gitlab.com/charts/installation/deployment.html
   - https://docs.gitlab.com/charts/installation/command-line-options.html
   - https://github.com/jenkinsci/helm-charts
   - https://github.com/jenkinsci/helm-charts/blob/main/charts/jenkins/README.md
   - https://github.com/jenkinsci/helm-charts/blob/main/charts/jenkins/values.yaml
   - https://docs.n8n.io/hosting/
   - https://docs.n8n.io/hosting/installation/docker/
   - https://github.com/8gears/n8n-helm-chart
   - https://developer.hashicorp.com/vault/docs/platform/k8s/helm
   
helm upgrade --install gitlab gitlab/gitlab \
  --timeout 600s \
  --set global.hosts.domain=genilsoncruz.com.br \
  --set global.hosts.externalIP=146.190.40.24 \
  --set certmanager-issuer.email=genilsoncruz@genilsoncruz.com.br \
  --set postgresql.image.tag=13.6.0 \
  --set global.edition=ce
  
helm uninstall gitlab gitlab/gitlab
  
helm show values jenkins/jenkins
  
helm upgrade --install jenkins --namespace jenkins -f values.yaml jenkinsci/jenkins;
    
helm uninstall jenkins jenkins/jenkins