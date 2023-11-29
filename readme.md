# AWS EKS

<details>
<summary> EXERCISE 1: Create EKS cluster 1
</summary>
<br>
You decide to create an EKS cluster - the managed Kubernetes Service of AWS. To simplify the whole creation and configurations, you use eksctl.With eksctl you create an EKS cluster with 3 Nodes and 1 Fargate profile

### Solution:
# 1. Install eksctl   
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
aws eks update-kubeconfig --name eks-cluster-test demo-cluster

# Validate:
cat ./kube/config
```

#### Deletion of cluster:
<code>eksctl delete cluster --name demo-cluster</code>
</details>


<details>
<summary> EXERCISE 2: Deploy Mysql and phpmyadmin
</summary>
<br>
You deploy mysql and phpmyadmin on EC2 nodes with the same setup as before.
</details>


<details>
<summary> EXERCISE 3: Deploy your Java application
</summary>
<br>
You deploy your Java application using Fargate with 3 replicas and same setup as before
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
