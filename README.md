# test_result

Assumptions
POD CIDR 10.244.0.0/16
Service CIDR : Auto

Spin up three servers, all the same type, with the Ubuntu 16.04 Xenial LTS distribution

    1. KubeMaster
    2. KubeWorker-0
    3. KubeWorker-1

Add  gpg key for the docker repository:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

Add Docker to  repository:
sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
 $(lsb_release -cs) \
 stable"

Add  gpg key for Kubernetes, and then add the repository:

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list 
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

apt-get update

Install docker kubelet, kubeadm, kubectl
apt-get install -y docker-ce kubelet kubeadm kubectl
apt-mark hold docker-ce kubelet kubeadm kubectl

kubelet is the node agent that will run all the pods for us, including the kube-system pods. The kubeadm is a tool for deploying multi-node kubernetes clusters. And then the kubectl is the command line tool for interacting with Kubernetes.


Enable the proper CGROUP driver:
CGROUP_DRIVER=$(sudo docker info | grep "Cgroup Driver" | awk '{print $3}')
echo $CGROUP_DRIVER
sed -i "s|KUBELET_KUBECONFIG_ARGS=|KUBELET_KUBECONFIG_ARGS=--cgroup-driver=$CGROUP_DRIVER |g" /etc/systemd/system/kubelet.service.d/10-kubeadm.conf


Now install commands on only the master server

Initialize the Cluster
kubeadm init --pod-network-cidr=10.244.0.0/16 –ignore-preflight-errors=NumCPU

X-------------------------------X
Your Kubernetes control-plane has initialized successfully!

To start using  cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 207.154.245.103:6443 --token vv97xy.pu1fjwfkuxwmkuem \
    --discovery-token-ca-cert-hash sha256:6b6f94e1bbe2727e094de97fd2743d18b60663ff0e0dc7b6e3b6283ad081b1ef 
X-------------------------------X



mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config


Now deploy a pod network to the cluster. Let’s apply our flannel CNI. Flannel is our network overlay, so our nodes can communicate with each other.

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml



On worker nodes run
kubeadm join 207.154.245.103:6443 --token vv97xy.pu1fjwfkuxwmkuem --discovery-token-ca-cert-hash sha256:6b6f94e1bbe2727e094de97fd2743d18b60663ff0e0dc7b6e3b6283ad081b1ef 



root@KubeMaster:~# kubectl get nodes
NAME           STATUS   ROLES    AGE    VERSION
kubemaster     Ready    master   21m    v1.15.1
kubeworker-0   Ready    <none>   115s   v1.15.1
kubeworker-1   Ready    <none>   104s   v1.15.1



Installing Helm
snap install helm --classic

vi rback-config.yaml

apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
kubectl create -f rbac-config.yaml

helm init --service-account tiller --history-max 200
