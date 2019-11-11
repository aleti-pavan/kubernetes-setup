# Setting up Kubernetes Environment

## Prerequisites

  You should have vagrant installed.

### Steps


    1. Get into current directory

    2. `vagrant up`  (creates the infrastructure)

    3. `vagrant status` (show machines created)

    4. `vagrant ssh k8s-master` ( which would take you to k8s master node )

    5. `kubectl get nodes`  ( should show you the nodes )

    6. 'kubectl cluster-info'
