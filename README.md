# RabbitMQ on Kubernetes!
Deploy a highly available RabbitMQ Stateful Cluster on Kubernetes.

## Gettings started:

Clone the repo:

```
git clone 'https://github.com/sabershahhoseini/rabbitmq-statefulset-cluster'
```

Create a namespace:

```
kubectl create ns rabbits
```

Go to **./manifests** directory and apply the manifests:

```
kubectl apply -n rabbits -f rabbit-rbac.yaml
```

```
kubectl apply -n rabbits -f rabbit-secret.yaml
```

```
kubectl apply -n rabbits -f rabbit-configmap.yaml
```

```
kubectl apply -n rabbits -f rabbit-statefulset.yaml
```

Wait for the cluster to be deployed.

```
watch kubectl -n rabbits get pods
```

Once all the replicas are running, run a shell inside one of them:

```
kubectl -n rabbits exec -it rabbitmq-0 bash
```

Once inside, use `rabbitmqctl` to set a policy for high availability and sync mode:

```
rabbitmqctl set_policy ha-fed ".*" '{"federation-upstream-set":"all", "ha-sync-mode":"automatic", "ha-mode":"nodes", "ha-params":["rabbit@rabbitmq-0.rabbitmq.rabbits.svc.cluster.local","rabbit@rabbitmq-1.rabbitmq.rabbits.svc.cluster.local","rabbit@rabbitmq-2.rabbitmq.rabbits.svc.cluster.local"]}' --priority 1 --apply-to queues
```
    
Believe it or not, you're done!
