# AWS EKS
Useful commands:
- <code>watch kubectl get pods,services,deployments --all-namespaces -o wide</code>
- <code>kubectl rollout restart deployment NAME_DEPLOYMENT -n NAMESPACE </code>
- <code>kubectl get events --field-selector involvedObject.name=POD_NAME -n NAMESPACE </code>


<details>
<summary> EXERCISE 1: Create EKS cluster 
</summary>
<br>
You decide to create an EKS cluster - the managed Kubernetes Service of AWS. To simplify the whole creation and configurations, you use eksctl.With eksctl you create an EKS cluster with 3 Nodes and 1 Fargate profile

#### Solution:
#### 1. Install eksctl   
```sh
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl
```

#### 2. Create Clsuter
***Create yaml file and use aws EKS CLI***

``` sh
# Create YAML (cluster.yaml)

apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: jane-cluster
  region: eu-west-3

nodeGroups:
  - name: my-node-group
    desiredCapacity: 3
    instanceType: t2.micro

fargateProfiles:
  - name: my-fargate-profile
    selectors:
      - namespace: my-app

# Create Cluster
eksctl create cluster -f cluster.yaml
```

***OR only use EKS CLI Commands***
```sh
# create cluster with 3 EC2 instances and store access configuration to cluster in kubeconfig.jane-cluster.yaml file 
eksctl create cluster --name=jane-cluster --nodes=3 --kubeconfig=./kubeconfig.jane-cluster.yaml

# create fargate profile in the cluster. It will apply for all K8s components in my-app namespace
eksctl create fargateprofile --cluster jane-cluster --name my-fargate-profile --namespace my-app
```

#### 3. Configure kubectl to connect to the cluster
```sh
# Check region (needs to be the same as in our cluster)
aws config list

# Create kubeconfig file (with information on how to connect to our cluster)
aws eks --region <your-region> update-kubeconfig --name <your-cluster-name>
aws eks update-kubeconfig --name jane-cluster

# Validate:
cat /Users/jfoerster008/.kube/config
```

#### 4. Validate that cluster got created
```sh
kubectl get node
eksctl get fargateprofile --cluster jane-cluster
```

#### Deletion of cluster:
<code>eksctl delete cluster --name jane-cluster</code>
</details>


<details>
<summary> EXERCISE 2: Deploy Mysql and phpmyadmin
</summary>
<br>
You deploy mysql and phpmyadmin on EC2 nodes with the same setup as before.
<br>
  
#### Solution:
  
#### 1. Point kubectl to your cluster 
<code>export KUBECONFIG=/Users/jfoerster008/.kube/config</code>

#### 2. Use Helm Charts to create 3 SQL Instances
```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
helm search repo bitnami/
helm install mysql bitnami/mysql -f sql-replica.yaml
```
#### 3. Deploy phpmyadmin instance using yaml files on EC2 Node
<code>kubectl apply -f mysql-secret.yaml</code><br>
<code>kubectl apply -f mysql-configmap.yaml</code><br>
<code>kubectl apply -f phpmyadmin.yaml</code><br>

#### 4. Port-forward traffic coming to localhost
Forward traffic from your local machine's port 8081 to the phpmyadmin-service service's port 8081.
<code>kubectl port forward svc/phpmyadmin-service 8081:8081</code> <br>

#### 5. Access phomyadmin in browser on
<code>localhost:8081</code> <br>

</details>


<details>
<summary> EXERCISE 3: Deploy your Java application
</summary>
<br>
You deploy your Java application using Fargate with 3 replicas and same setup as before

#### Solution:

#### 1. You need to create a new namespace for the fargate profile:
<code>kubectl create namespace my-app</code><br>

#### 2. Create Key (login to Registry and create Secret in K8S)

```sh
DOCKER_REGISTRY_SERVER=https://index.docker.io/v1/
DOCKER_USER=your docker username
DOCKER_EMAIL=your dockerhub email
DOCKER_PASSWORD= dockerhub pwd

kubectl create secret docker-registry my-registry-key1 \
--docker-server=<DOCKER_REGISTRY_SERVER>
--docker-username=<DOCKERHUB_USERNAME> \
--docker-password=<DOCKERHUB_PASSWORD> \
--docker-email=<DOCKERHUB_EMAIL>
```

#### 3. Execute following commands
<code>kubectl apply -f mysql-secret.yaml</code><br>
<code>kubectl apply -f mysql-configmap.yaml</code><br>
<code>kubectl apply -f deployment.yaml</code><br>


*** Fix: CrashLoop for Java APP!!! ***
</details>


<details>
<summary> EXERCISE 4
</summary>
</details>


<details>
<summary> EXERCISE 5
</summary>
</details>


<details>
<summary> EXERCISE 6
</summary>
</details>
