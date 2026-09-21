<img width="1130" height="386" alt="Screenshot 2026-09-21 120043" src="https://github.com/user-attachments/assets/0e53092c-a5e0-47d7-809f-d775d9c3856f" />
1. Logs (Application Output)
Logs are the standard output (stdout) and standard error (stderr) streams emitted by the code running inside your containers.

Source: Your application (Node.js, Python, Java, Nginx, etc.).

Purpose: Debugging application-level behavior (e.g., tracking a user request, finding a stack trace for an API error, or checking database connection attempts).

Lifespan: Depends on your node's log rotation settings or your external log aggregation tool (like ElasticSearch or Splunk).

Kubernetes Commands for Logs:

View basic logs for a pod:

Bash
kubectl logs <pod-name>
Stream (tail) logs in real-time:

Bash
kubectl logs -f <pod-name>
View logs for a specific container (if a pod has multiple containers):

Bash
kubectl logs <pod-name> -c <container-name>
View logs from a previously crashed container (crucial for figuring out why a pod restarted):

Bash
kubectl logs --previous <pod-name>
2. Events (Cluster Activity)
Events are actual Kubernetes resource objects that report what the cluster control plane is doing or what is happening to your resources.

Source: Kubernetes components (Kubelet, Scheduler, Controllers).

Purpose: Debugging cluster-level and infrastructure issues (e.g., node autoscaling, pod scheduling failures, image pull errors like ErrImagePull, or readiness probe failures).

Lifespan: Events are highly transient. To prevent the cluster's database (etcd) from filling up, Kubernetes usually deletes events after 1 hour.

Kubernetes Commands for Events:

View all events in the current namespace:

Bash
kubectl get events
View events sorted by time (highly recommended, as default sorting can be confusing):

Bash
kubectl get events --sort-by='.lastTimestamp'
Watch events in real-time:

Bash
kubectl get events -w
View events for a specific resource:
The most common way to view events for a specific pod, deployment, or node is to use the describe command. The events will be listed at the very bottom of the output.

Bash
kubectl describe pod <pod-name>
