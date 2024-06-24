### Summary

### Requesting Resources
- The _minimum_ amount of resources your pod needs, not the _max_.
- Specified for each container in a Pod.
- `cpu: 200m` means 200 milicores which means 1/5 of a cpu. 

```bash
k exec -it requests-pod top
```

- `top` can give you the CPU consumption.
- The scheduler takes this into account when assigning a pod to a node.
- Scheduling always works on how much was _actually_ allotted not how much is currently being used. 
	- So if a node is technically 80% full but its only really using 70% and a new pod requests at least 25%, it can't be scheduled to that node.
- Scheduler prioritizes nodes with heavy requests on them to bundle pods tightly to hopefully free up and remove unused nodes.
- If the scheduler can't fulfill your resource requests on any node, the pod will remain `Pending`.
- Any free cpu is split up accordingly between the pods on the node.

>![[Pasted image 20240624115700.png]]

### Limiting Resources
- You should limit the memory given to a container, because that cannot be taken back like the cpu can.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: limited-pod
spec:
  containers:
  - image: busybox
    command: ["dd", "if=/dev/zero", "of=/dev/null"]
    name: main
	resources:
	  limits:
		cpu: 1
		memory: 20Mi
```

- Sum of all resource limits are _allowed_ to exceed 100% of the nodes capacity.
- CPU is throttled, so it cannot exceed its limit.
- If a process tries to exceed its memory allocated, it is killed and restarted.
- `OOMKilled` means a pod was killed because "Out of Memory".
- You can see why a pod was killed by doing a `describe` on it.
- `top` command shows the memory and cpu amounts of the whole node the container is running on.
- 