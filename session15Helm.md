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
<img width="1184" height="268" alt="18-mini-project-cleanup" src="https://github.com/user-attachments/assets/9e1e2b7b-d853-4ca2-9a2d-abfcd8a7ef92" />
<img width="1184" height="540" alt="17-mini-project-break-rollback" src="https://github.com/user-attachments/assets/428d802d-78f0-428e-af9b-02f503e758ee" />
<img width="1184" height="404" alt="16-mini-project-upgrade-prod" src="https://github.com/user-attachments/assets/67797ff9-0c40-439a-b82d-3d70ec12924e" />
<img width="1184" height="421" alt="15-mini-project-install" src="https://github.com/user-attachments/assets/dbbacb67-88fb-4486-ae22-5a73a595bd33" />
<img width="1184" height="1135" alt="14-mini-project-lint-template" src="https://github.com/user-attachments/assets/1579e694-610e-4857-90ca-ca470143fc8b" />
<img width="1184" height="557" alt="13-guestbook-upgrade-rollback" src="https://github.com/user-attachments/assets/feb1e906-0a50-4994-abdd-0c40226cc2d9" />
<img width="1184" height="506" alt="12-guestbook-lint-install" src="https://github.com/user-attachments/assets/63d5b212-dacc-4fa7-a429-7eec4aa8a4e8" />
<img width="1184" height="404" alt="11-atomic-rollback" src="https://github.com/user-attachments/assets/a84b34d8-a1dc-45d9-9210-cfa3574c5c4e" />
<img width="1184" height="676" alt="10-rollback" src="https://github.com/user-attachments/assets/666f084a-ecdf-4ef7-afb8-c64cfe6f1a26" />
<img width="1184" height="404" alt="09-upgrade-install-uninstall" src="https://github.com/user-attachments/assets/08db4d5d-4ba6-4550-8a38-46cefb1995e0" />
<img width="1184" height="625" alt="08-install-upgrade" src="https://github.com/user-attachments/assets/d479bec8-e227-47bd-b657-da9a65d43f32" />
<img width="1184" height="761" alt="07-templates" src="https://github.com/user-attachments/assets/1143c9d3-d868-4b0f-8ae0-ff8f4e1ef9b3" />
<img width="1184" height="455" alt="06-values-override" src="https://github.com/user-attachments/assets/8e07afab-fa43-484b-8f74-0c9ec2955a69" />
<img width="1184" height="421" alt="05-chart-yaml-lint" src="https://github.com/user-attachments/assets/b5a022c2-ae7c-4dde-a5e9-7f99aba03427" />
<img width="1184" height="931" alt="04-chart-structure" src="https://github.com/user-attachments/assets/24ee08b9-bc83-4e65-a647-d9e36d4c5265" />
<img width="1184" height="523" alt="03-install-demo-chart" src="https://github.com/user-attachments/assets/947842c9-e4ba-41d8-9484-23b5ac8b96c1" />
<img width="1184" height="1237" alt="02-create-chart" src="https://github.com/user-attachments/assets/dc5a1b76-9cd6-4587-9139-a0831eddfc77" />
<img width="1184" height="472" alt="01-helm-version" src="https://github.com/user-attachments/assets/7c80285a-5585-4eae-94de-a81dc364c2e5" />


