# Setting up Kubernetes Environment

## Prerequisites

  You should have vagrant installed.

### Steps


    1. Get into current directory

    2. `vagrant up`  (creates the infrastructure)

    3. `vagrant status` (show machines created)

    4. `vagrant ssh k8s-master` ( which would take you to k8s master node )

    5. `kubectl get nodes`  ( should show you the nodes )

    6. `kubectl cluster-info` ( shows you cluster-info )

    7. ``



# cluster installation: 

https://blog.exxactcorp.com/building-a-kubernetes-cluster-using-vagrant/

git clone https://exxsyseng@bitbucket.org/exxsyseng/k8s_centos.git      # Centos k8s Cluster
git clone https://exxsyseng@bitbucket.org/exxsyseng/k8s_ubuntu.git      # Ubuntu k8s Cluster

