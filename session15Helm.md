# Helm Rollback Workflow

## Objective
Demonstrate installing a Helm release, upgrading it twice, verifying the changes, and rolling back to a previous revision.

## Release
- Release name: `notes`
- Namespace: `default`
- Chart: `notes-chart-0.1.0`

## 1. Install
```bash
helm install notes ./mini-project/notes-chart
```
The installation succeeded with revision 1. The initial Deployment had one replica and used the development configuration.

## 2. Upgrade to production
```bash
helm upgrade notes ./mini-project/notes-chart \
  -f ./mini-project/notes-chart/values-prod.yaml
```
This created revision 2. The production configuration uses three replicas, NGINX 1.25, and `ENVIRONMENT=production`.

## 3. Verify the first upgrade
```bash
helm status notes
kubectl get deployment notes-deploy
kubectl get pods -l app=notes
kubectl get configmap notes-config -o yaml
```
The Deployment reached three available replicas and the ConfigMap showed the production environment.

## 4. Upgrade again
```bash
helm upgrade notes ./mini-project/notes-chart \
  -f ./mini-project/notes-chart/values-prod.yaml \
  --set replicaCount=2
```
This created revision 3 with a replica count of two.

## 5. Verify the second upgrade
```bash
helm history notes
kubectl get deployment notes-deploy
kubectl get pods -l app=notes
```

## 6. Roll back
```bash
helm rollback notes 2
```
Helm rolled the release back to the configuration stored in revision 2. The rollback itself created revision 4.

## 7. Verify the rollback
```bash
kubectl rollout status deployment/notes-deploy --timeout=120s
helm history notes
helm status notes
kubectl get deployment notes-deploy
kubectl get configmap notes-config -o yaml
```

## Final result
- Helm release status: `deployed`
- Current revision: 4
- Revision description: `Rollback to 2`
- Deployment availability: 3/3 replicas
- ConfigMap environment: `production`
- Rollout status: successful

## Evidence
Attach terminal screenshots showing the install, both upgrades, revision history, rollback, final Deployment status, and production ConfigMap.
<img width="1156" height="518" alt="Screenshot 2026-10-16 092132" src="https://github.com/user-attachments/assets/8f836c7b-f0f3-4abd-8772-e46cee2a3fdc" />
<img width="1031" height="1012" alt="Screenshot 2026-10-16 092124" src="https://github.com/user-attachments/assets/b6b6cdcd-89e4-4ad9-ae6a-6cd7932c29ef" />
<img width="1037" height="788" alt="Screenshot 2026-10-16 092115" src="https://github.com/user-attachments/assets/3df1483b-1a1a-46de-b319-a73fef2dc935" />
<img width="1276" height="967" alt="Screenshot 2026-10-16 092332" src="https://github.com/user-attachments/assets/7638f6d3-a94c-4cd8-90e4-bb8c5bd73f00" />
<img width="1156" height="1007" alt="Screenshot 2026-10-16 092304" src="https://github.com/user-attachments/assets/ed28ef34-297d-49b5-931e-51591708b4cd" />
<img width="1117" height="918" alt="Screenshot 2026-10-16 092257" src="https://github.com/user-attachments/assets/2ec06c57-1cfe-45b6-8951-5a565a7a470d" />

