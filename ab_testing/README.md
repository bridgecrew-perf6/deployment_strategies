# Recreate deployment

> Version A is terminated then version B is rolled out.

![kubernetes recreate deployment](grafana-recreate.png)

The recreate strategy is a dummy deployment which consists of shutting down
version A then deploying version B after version A is turned off. This technique
implies downtime of the service that depends on both shutdown and boot duration
of the application.

## Steps to follow

1. version 1 is service traffic
1. delete version 1
1. deploy version 2
1. wait until all replicas are ready

## In practice

```bash
# Deploy the first application
$ kubectl apply -f kuber-deployment-v1.yaml

# Test if the deployment was successful
$ curl $(minikube service kuber-service --url)

# To see the deployment in action, open a new terminal and run the following command
$ watch kubectl get po --watch

# Then deploy version 2 of the application
$ kubectl apply -f kuber-deployment-v2.yaml

# Test the second deployment progress
service=$(minikube service kuber-service --url)
while true; do curl "$service"; sleep 2; echo; done

# Cleanup
$ kubectl delete all -l app=my-app
```

## Visualize step by step 

### Init state

![recreate init state](./recreate_init_state.png)

### Step 1

![recreate step 1](./recreate_step_1.png)

### Step 2

![recreate step 2](./recreate_step_2.png)
