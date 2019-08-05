#
# Feedback on test: A working individual will not get 10 hours free time to work on such a test but can do all of the following tasks. Thanks for the questions.
#
1. Deploying Kubernetes and Helm in the file K8-3nodecluster.odt. Additional reference: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/
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
3. Deploying with helm: https://www.digitalocean.com/community/tutorials/how-to-install-software-on-kubernetes-clusters-with-the-helm-package-manager
4. Jenkins CICD : https://dzone.com/articles/setting-up-cicd-pipelines-for-docker-kubernetes-pr
5. Deploying Prometheus and Grafana : https://kubernetes.github.io/ingress-nginx/user-guide/monitoring/
6. Deploying ELK stack as log analysis/ SIEM tool: log can be written and shipped via syslog mechanism[ UDP 514 port]. Filebeat plugin will process and help extract meaningful data.Kibana is the dashboard. Elastic search does the indexing and stores the logs.Grok filter can be used to extract patterns.
7. Canary deployment: https://dzone.com/articles/a-detailed-guide-to-canary-deployments
    https://developer.ibm.com/tutorials/use-jenkins-and-istio-for-canary-deployment/
8.  Automate with wrapper: The same sequence of commands given in all above can be written using shell script or ansible playbook      by creating tasks inside playbooks
