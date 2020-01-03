# Kubernetes controllers

We are discussing 3 main types of controllers [here](https://www.mirantis.com/blog/kubernetes-replication-controller-replica-set-and-deployments-understanding-replication-options/).

1. ReplicationController

2. ReplicationSet

3. Deployment



differences b/w `ReplicationController` and `ReplicationSet`


|Replica Set|Replication Controller|
|-----------|----------------------|

| Replica Set supports the new set-based selector. | Replication Controller only supports equality-based |
| This gives more flexibility. for eg:             | selector. for eg:                                   |
|          environment in (production, qa)         |             environment = production                |
|  This selects all resources with key equal to    | This selects all resources with key equal to        |
|  environment and value equal to production or qa | environment and value equal to production           |
  rollout command is used for updating the replica set. | rolling-update command is used for updating   |
| Even though replica set can be used independently,    | the replication controller. This replaces the |
| it is best used along with deployments which          | specified replication controller with a new   |
| makes them declarative.                               | replication controller by updating one pod    |
|                                                       | at a time to use the new PodTemplate.         |
+-------------------------------------------------------+-----------------------------------------------+
