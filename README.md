# k8s

k8s

## Tutorial

- [https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.2/examples/echo_server/](https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.2/examples/echo_server/)
- [https://helm.sh/](https://helm.sh/)
- [https://kustomize.io/](https://kustomize.io/)

## How to setup

- Configure aws cli with reviewty-prod account, ap-southeast-1 region.

- Create cluster:

```
eksctl create cluster -f cluster.yaml
```

- Create namespace:

```
eksctl apply -f namespace.yaml
```

- Create secret:

```
kubectl apply -f api/api-secret.yaml
kubectl create secret generic firebase-adminsdk -n production --from-file=api/firebase-adminsdk.json
kubectl create secret generic apple-private-key -n production --from-file=web/AuthKey_935CY3T4J8.p8
```

Should use kustomize to describe secret.

-

```
kubectl apply -f api/api-configmap.yaml && \
kubectl apply -f api/api-service.yaml && \
kubectl apply -f api/api-deployment.yaml
```

Do the simillar commands for web.

- Setup AWS load balancer controller: [https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.2/examples/echo_server/](https://kubernetes-sigs.github.io/aws-load-balancer-controller/v2.2/examples/echo_server/)

- Enable metric server for cluster then apply HPA:
  [https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html](https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html)
  [https://github.com/kubernetes-sigs/metrics-server#deployment](https://github.com/kubernetes-sigs/metrics-server#deployment)

  ```
  kubectl apply -f api/api-hpa.yaml
  ```

- Update path of health checks of target groups to /health-check

- Setup Cluster Autoscaler:
  [https://docs.aws.amazon.com/eks/latest/userguide/cluster-autoscaler.html#ca-deploy](https://docs.aws.amazon.com/eks/latest/userguide/cluster-autoscaler.html#ca-deploy)

# Task

- Migrate database: using AWS DMS:
  ERROR: type "geography" does not exist; Error while executing the query
- Migrate elasticsearch: [https://aws.amazon.com/premiumsupport/knowledge-center/migrate-amazon-es-domain/](https://aws.amazon.com/premiumsupport/knowledge-center/migrate-amazon-es-domain/)
  Or just delete the old one and create a new one.


# EKS CMD

kubectl edit deployments/api -n production
kubectl logs -f -l app=api -n production --max-log-requests=20
kubectl get pods -n production

Then check the status and history using the below commands,

$ kubectl rollout status deploy myapp-deployment
$ kubectl rollout history deploy myapp-deployment

