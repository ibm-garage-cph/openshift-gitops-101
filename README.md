# openshift-gitops-101

## 1. Install operator

1. Create a project named `argocd`

2. With the `argocd` project selected, go to the operator hub and search for `argo`

3. Select the Argo CD operator

![step1](/images/step1.1.png)

*Note: use the ArgoCD provided operator*

4. Select `Continue` and `Install`

5. On the `Create Operator Subscription` screen, select the installation namespace to be `argocd` (your newly created project)

![step1](/images/step1.2.png)

## 2. Wait until installed

Normally a minute or two until installation is complete. A good indication is when the pods in the `argocd` namespace are finalized and stable (1/1)

## 3. Enable the ArgoCD UI

1. Take the yaml located [here](./1_argo-cd.yaml) and apply it inside the `argocd` namespace

```bash
# selecting project
oc project argocd

# applying ArgoCD resource
oc apply -f ./0_argo-cd.yaml
```

2. There will be several Argo CD resources created that should be familiar to anyone who has deployed Argo CD.

```bash
oc get cm,secret,deploy
```

3. When everything is installed, you can fetch argocd's URLs:

```bash
oc get routes
``` 

4. Open the route URL in a browser

5. The cluster Secret contains the admin password for authenticating with Argo CD, as well as the Grafana dashboard, if enabled. Fetch the admin password from the cluster Secret.

```bash
kubectl get secret example-argocd-cluster -o jsonpath='{.data.admin\.password}' | base64 -d
```
6. You can log into the web UI with the username `admin` and the password extracted from the step above

![step1](/images/step2.png)

7. You should now be in the main dashboard


## 4. Create you application

- Click on `NEW APP`.
- Add the below details:
  - Application Name: `sample`
  - Project: `default`
  - SYNC POLICY: `Manual`
  - REPO URL: `https://github.com/ibm-garage-cph/openshift-gitops-101`
  - Revision: `HEAD`
  - Path: `openshift`

![app details one](/images/app_argo_1.png)

- Cluster - Select the default one `https://kubernetes.default.svc` to deploy in-cluster
- Namespace - `dev-mn` (or your initials)
- Click Create to finish

![app details two](/images/app_argo_2.png)

- You will now see the available apps.

![sampleapp create](/images/sampleapp_create.png)


![sampleapp create](./images/sampleapp_create.png)

- Initially, the app will be out of sync. It is yet to be deployed. You need to sync it for deploying.

To sync the application, click `SYNC` and then `SYNCHRONIZE`.

![out of sync](./images/out_of_sync.png)

- Wait till the app is deployed.

![synched app](./images/synched_app.png)

- Once the app is deployed, click on it to see the details.

![sample app deployed](./images/sample_app_deployed.png)

![sample app full deployment](./images/sample_app_full_deployment.png)