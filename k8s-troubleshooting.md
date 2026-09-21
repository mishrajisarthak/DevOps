<img width="1443" height="848" alt="Screenshot 2026-09-21 124019" src="https://github.com/user-attachments/assets/2e8fddd4-c74e-4cb7-a7c8-0afc3d21437a" />
<img width="1429" height="850" alt="Screenshot 2026-09-21 124010" src="https://github.com/user-attachments/assets/6b36ca74-c4d6-45c0-abc6-01afe9ac3b93" />
<img width="1419" height="854" alt="Screenshot 2026-09-21 124001" src="https://github.com/user-attachments/assets/0ab39188-d7b1-488f-8644-5283766a1a38" />
<img width="1421" height="843" alt="Screenshot 2026-09-21 123953" src="https://github.com/user-attachments/assets/a30d3cc4-0f02-424c-98ac-846609b62b96" />
<img width="1410" height="845" alt="Screenshot 2026-09-21 123944" src="https://github.com/user-attachments/assets/5b8022aa-f3d0-49c6-a4c8-98be5a1d9c4b" />
<img width="1405" height="817" alt="Screenshot 2026-09-21 123933" src="https://github.com/user-attachments/assets/845baeaf-9fb3-4945-86c5-3449dc70b438" />
<img width="1410" height="836" alt="Screenshot 2026-09-21 123923" src="https://github.com/user-attachments/assets/4bccfffe-ff2d-4c2d-8d64-bac6de567397" />
<img width="1410" height="799" alt="Screenshot 2026-09-21 123913" src="https://github.com/user-attachments/assets/237eec67-3d9c-43ba-abe0-1336e2a00f32" />
<img width="1386" height="798" alt="Screenshot 2026-09-21 123901" src="https://github.com/user-attachments/assets/12608069-2f43-434f-96f0-9e31970ce8f1" />
<img width="1408" height="805" alt="Screenshot 2026-09-21 123852" src="https://github.com/user-attachments/assets/8fc627ea-16d6-4c19-b3bd-f01dcfb3e5dc" />
<img width="1461" height="913" alt="Screenshot 2026-09-21 124100" src="https://github.com/user-attachments/assets/604605ba-c090-499b-8156-75143eca771d" />
<img width="1461" height="913" alt="Screenshot 2026-09-21 124053" src="https://github.com/user-attachments/assets/f0e3fe14-cab4-40a2-9220-790bb3710b2f" />
<img width="1461" height="913" alt="Screenshot 2026-09-21 124046" src="https://github.com/user-attachments/assets/dfe5f7cd-9713-457d-9373-38139fd4dbc0" />
<img width="1457" height="890" alt="Screenshot 2026-09-21 124036" src="https://github.com/user-attachments/assets/7fd6c81d-952c-4279-94ff-4d46a1a2aa91" />
<img width="1461" height="913" alt="Screenshot 2026-09-21 124028" src="https://github.com/user-attachments/assets/3cdef335-ffea-412b-ad98-5f34f69389dd" />
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
