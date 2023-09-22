# kubernetes-scale

# eksctl
##### for ARM systems, set ARCH to: `arm64`, `armv6` or `armv7`
ARCH=amd64
PLATFORM=$(uname -s)_$ARCH

curl -sLO "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$PLATFORM.tar.gz"

##### (Optional) Verify checksum
curl -sL "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_checksums.txt" | grep $PLATFORM | sha256sum --check

tar -xzf eksctl_$PLATFORM.tar.gz -C /tmp && rm eksctl_$PLATFORM.tar.gz

sudo mv /tmp/eksctl /usr/local/bin

# kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

kubectl version --client

# AWS EKS Cluster Auto Scalling

1. **Command**: `eksctl create cluster --name eks-demo-cluster`
    - **Description**: Create an Amazon EKS cluster named "eks-demo-cluster."
2. **Command**: `kubectl get deployment metric-server -n kube-system`
    - **Description**: Retrieve information about the "metric-server" deployment in the "kube-system" namespace.
3. **Command**: `kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`
    - **Description**: Deploy the Metrics Server for Kubernetes from a YAML configuration file.
4. **Command**: `kubectl get deployment metrics-server -n kube-system`
    - **Description**: Retrieve information about the "metric-server" deployment in the "kube-system" namespace.
5. **Command**: `kubectl apply -f https://k8s.io/examples/application/php-apache.yaml`
    - **Description**: Deploy the PHP Apache application from a YAML configuration file.
6. **Command**: `kubectl get all`
    - **Description**: List all Kubernetes resources in the current context and namespace.
7. **Command**: `kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- <http://php-apache>; done"`
    - **Description**: Run a load generator using a busybox container to continuously access the "php-apache" service.
8. **Command**: `kubectl get hpa`
    - **Description**: Get Horizontal Pod Autoscalers (HPA) in the current context and namespace.
9. **Command**: `kubectl get rs`
    - **Description**: Get ReplicaSets (RS) in the current context and namespace.
10. **Command**: `kubectl get pods`
    - **Description**: Get pods in the current context and namespace.

