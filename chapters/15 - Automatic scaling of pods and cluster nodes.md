### Summary


### Horizontal pod autoscaling
- Performed by Horizontal controller which is configured by a HorizontalPodAutoscaler (HPA).
- Uses heapster to get its stats.
- Calculating the required replica count is simple when it considers only a single metric.

```ad-example
Pod 1: 60% CPU utilization
Pod 2: 90% CPU utilization
Pod 3: 50% CPU utilization
Target Utilization: 50% CPU
(60 + 90 + 50) / 50 = 200 / 50 = 4 (replicas)
```

- HPA only modifies on the Scale sub-resource.
- This allows it to operate on any scalable resource (e.g. pods, deployments, statefulsets).
- Autoscaler compares a pods actual cpu consumption and its requested amount so pods will need to have that set.
- `autoscale` command can create a HPA quickly.

```bash
$ k autoscale deployment kubia --cpu-percentage=30 --min=1 --max=5
```

- 