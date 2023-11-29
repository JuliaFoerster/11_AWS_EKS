# AWS EKS

<details>
<summary> EXERCISE 1: Create EKS cluster 1
</summary>
<br>
You decide to create an EKS cluster - the managed Kubernetes Service of AWS. To simplify the whole creation and configurations, you use eksctl.With eksctl you create an EKS cluster with 3 Nodes and 1 Fargate profile

#### Solution:
#### 1. Install eksctl   
```sh
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl
```

#### 2. Create YAML (cluster.yaml)
``` sh
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: demo-cluster
  region: eu-west-3

nodeGroups:
  - name: node-group-1
    desiredCapacity: 3
    instanceType: t2.micro

fargateProfiles:
  - name: fargate-profile-1
    selectors:
      - namespace: default
```

#### 3. Create Cluster using eksctl 
<code>eksctl create cluster -f cluster.yaml</code>

#### 4. Configure kubectl to connect to the cluster
```sh
# Check region (needs to be the same as in our cluster)
aws config list

# Create kubeconfig file (with information on how to connect to our cluster)
aws eks --region <your-region> update-kubeconfig --name <your-cluster-name>
aws eks update-kubeconfig --name demo-cluster

# Validate:
cat /Users/jfoerster008/.kube/config
```

#### 5. Validate that cluster got created
```sh
kubectl get node
eksctl get fargateprofile --cluster demo-cluster
```

#### Deletion of cluster:
<code>eksctl delete cluster --name demo-cluster</code>
</details>


<details>
<summary> EXERCISE 2: Deploy Mysql and phpmyadmin
</summary>
<br>
You deploy mysql and phpmyadmin on EC2 nodes with the same setup as before.
<br>

#### 1. Point kubectl to your cluster 
export KUBECONFIG=/Users/jfoerster008/.kube/config

#### 2. Use Helm Charts to create 3 SQL Instances
```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
helm search repo bitnami/
helm install mysql bitnami/mysql -f sql-replica.yaml
```

#### 3. Deploy phpmyadmin instance on EC2 Node
```sh
kubectl apply -f mysql-configmap.yaml
kubectl apply -f mysql-secret.yaml
kubectl apply -f phpmyadmin.yaml
```
</details>


<details>
<summary> EXERCISE 3: Deploy your Java application
</summary>
<br>
You deploy your Java application using Fargate with 3 replicas and same setup as before

#### Solution


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
