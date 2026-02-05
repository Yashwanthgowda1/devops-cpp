### Installation of Kubernetes

We can use either **minikube** or **kind** to work with Kubernetes locally.

## KIND (Kubernetes IN Docker) Installation

KIND is a tool for running local Kubernetes clusters using Docker container nodes.

### Prerequisites
- Docker installed and running
- Linux/macOS/Windows with Docker support

### Installation Steps

#### 1. Download KIND Binary

**Linux & macOS:**
```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

**macOS (using Homebrew):**
```bash
brew install kind
```

**Windows (using Chocolatey):**
```bash
choco install kind
```

#### 2. Verify Installation
```bash
kind version
```

#### 3. Create a Kubernetes Cluster
```bash
# Create a basic cluster named "kind-cluster"
kind create cluster --name kind-cluster

# Create cluster with specific Kubernetes version
kind create cluster --name kind-cluster --image kindest/node:v1.27.0

# Create multi-node cluster
kind create cluster --name multi-node --config kind-config.yaml
```

#### 4. Set kubectl Context
```bash
# Automatically uses the new cluster context
kubectl cluster-info --context kind-kind-cluster

# Verify connection
kubectl get nodes
```

#### 5. Load Docker Images into KIND
```bash
# Build or tag your image
docker build -t my-app:latest .

# Load into KIND cluster
kind load docker-image my-app:latest --name kind-cluster
```

### Useful Commands

```bash
# List clusters
kind get clusters

# Get cluster info
kubectl cluster-info --context kind-kind-cluster

# Delete cluster
kind delete cluster --name kind-cluster

# View logs
kind export logs ./logs-dir
```

### Sample Multi-Node Configuration (kind-config.yaml)
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

### Troubleshooting (KIND)

- **Docker not running**: Ensure Docker daemon is started
- **Insufficient resources**: KIND requires ~2GB RAM per node
- **Port conflicts**: Ensure ports 6443 (API server) are free

---

## Minikube Installation

Minikube is a tool that runs a single-node Kubernetes cluster in a Virtual Machine (VM) on your personal computer.

### Prerequisites
- Virtualization manager: Docker, VirtualBox, KVM, Hyper-V, or Podman
- At least 2 CPUs and 2GB RAM
- Internet connection

### Installation Steps

#### 1. Download Minikube Binary

**Linux:**
```bash
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

**macOS (Intel):**
```bash
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
```

**macOS (Apple Silicon):**
```bash
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-darwin-arm64
sudo install minikube-darwin-arm64 /usr/local/bin/minikube
```

**macOS (using Homebrew):**
```bash
brew install minikube
```

**Windows (using Chocolatey):**
```bash
choco install minikube
```

**Windows (using Scoop):**
```bash
scoop install minikube
```

#### 2. Verify Installation
```bash
minikube version
```

#### 3. Start Minikube Cluster
```bash
# Start with default settings (Docker driver)
minikube start

# Start with specific VM driver
minikube start --driver=virtualbox
minikube start --driver=kvm2
minikube start --driver=hyperv

# Start with custom resources
minikube start --cpus=4 --memory=8192

# Start with specific Kubernetes version
minikube start --kubernetes-version=v1.27.0
```

#### 4. Access the Cluster
```bash
# Get cluster info
minikube status

# SSH into the node
minikube ssh

# Access Kubernetes dashboard
minikube dashboard

# Get kubectl context
kubectl config current-context

# Verify connection
kubectl get nodes
```

#### 5. Configure kubectl
```bash
# The context is automatically set, but you can set it manually
kubectl config use-context minikube

# Get cluster details
kubectl cluster-info
```

#### 6. Build and Run Docker Images
```bash
# Use Minikube's Docker daemon
eval $(minikube docker-env)

# Build image directly in Minikube
docker build -t my-app:latest .

# Deploy to Minikube
kubectl run myapp --image=my-app:latest
```

### Useful Commands

```bash
# Get cluster status
minikube status

# Stop cluster
minikube stop

# Delete cluster
minikube delete

# Get minikube IP
minikube ip

# List clusters
minikube profile list

# Add-ons management
minikube addons list
minikube addons enable ingress
minikube addons enable metrics-server

# Mount directory from host
minikube mount /host/path:/vm/path

# View logs
minikube logs

# Get minikube service URL
minikube service <service-name>
```

### Sample Deployment for Testing
```bash
# Create a simple deployment
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0

# Expose as service
kubectl expose deployment hello-minikube --type=NodePort --port=8080

# Get service URL
minikube service hello-minikube

# Clean up
kubectl delete deployment hello-minikube
kubectl delete service hello-minikube
```

### Troubleshooting (Minikube)

- **VM driver not found**: Install appropriate hypervisor (VirtualBox, KVM, etc.)
- **Insufficient resources**: Increase VM memory: `minikube config set memory 4096`
- **Network issues**: Try `minikube delete` and `minikube start` again
- **Docker daemon conflicts**: Use `eval $(minikube docker-env)` to use Minikube's Docker
- **Dashboard not accessible**: Run `minikube dashboard` to open browser automatically






###  distrubution to handel this kubernaties
grander 
redhat   --> openshift
amazon linux  ---> eks
cantos
ranchues
git 




---------------------------------------------------------------------------------------

###### KOPS mostly using to steup the things fallow this cmd

kops is only a cluster provisioning tool.   


conigure the first aws 

## create a bucket
 aws s3api create-bucket --bucket <kops-yash-storage-forkubernatis> --region <us-east-1>

## createa clusetr using the storegare
    export KOPS_STATE_STORE=s3://kops-yash-storage-forkubernatis

    verify the the bucket name
    echo $KOPS_STATE_STORE


kops create cluster --name=<k8scluster.k8s.local_domain_name_also> --zones=<us-east-1a> --node-count=1 --node-size=t2.micro --control-plane-size=t2.micro --control-plane-volume-size=8 --node-volume-size=8


### Real Kubernetes workflow (after cluster is ready)

Once you run:

kops update cluster --yes


and validate:

kops validate cluster


You switch to Kubernetes commands.

after that normally write the yaml and handel the things
