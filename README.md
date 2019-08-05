#
# Feedback on test: For a working individual will not get 10 hours in a week to work on such a test. I work as architect and work on RFP replies due to which I get very less time free even during weekends. Thanks for the questions.
#
1. Deploying Kubernetes and Helm in the K8-3nodecluster.odt. Additional reference: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/
2. Create a namespace: 
create a file dev_namespace.json with following content: 
{
  "apiVersion": "v1",
  "kind": "Namespace",
  "metadata": {
    "name": "development",
    "labels": {
      "name": "development"
    }
  }
}

kubectl create -f dev_namespace.json

2. Deploying guest-book: https://kubernetes.io/docs/tutorials/stateless-application/guestbook/
3. Jenkins CICD : https://dzone.com/articles/setting-up-cicd-pipelines-for-docker-kubernetes-pr
4. Deploying Prometheus and Grafana : https://kubernetes.github.io/ingress-nginx/user-guide/monitoring/
