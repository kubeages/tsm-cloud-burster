# tsm-cloud-burster v1.0

K8s app for performing automatic cloud bursting

(For educational/demo purposes only)


![alt](https://github.com/kubeages/tsm-cloud-burster/blob/main/automatic-cloud-bursting.png)

## Building the image

Inside the folder Dockerfile, you can find the file to build the image. Inside the Dockerfile, replace KUBE_LATEST_VERSION with the version of the K8s cluster where you'll deploy this app.

Steps to build the image if you use Docker and you have logged in already into <YOUR_REGISTRY>:

- build the image

> `docker build . -t <YOUR_REGISTRY>/tsm-cloud-burster`

- push the image

> `docker push <YOUR_REGISTRY>/tsm-cloud-burster`


If you don't want or cannot build the image, you can use this image:
**tanzueseemea/tsm-cloud-burster**


## Customizing the K8s deployment

Inside the folder K8s, you'll find a single file yaml (called tsm-cloud-burster.yaml) with all the components you'll need to deploy tsm-cloud-burster app. Some considerations:
- Replace <LOCAL_NAMESPACE_WHERE_APP_IS_RUNNING> with the namespace of the K8s cluster where the app is currently running
- Replace <REMOTE_NS_WHERE_TO_BURST_THE_APP> with the namespace of the remote cloud-based K8s cluster where the app will be scaled in case of cloud bursting
- Replace <REMOTE_DEPLOYMENT_TO_SCALE_WHEN_BURSTING> with the name of the deployment of the app running on the remote cloud-based K8s cluster (typically the same deployment that is running in the local K8s cluster). Note: this deployment should be scaled to 0 to show better the cloud-bursting effect.
- Replace <TSM_AUTOSCALER_NAME> with the name of the TSM autoscaler deployed on the same the namespace of the app running in the local K8s cluster
- Replace <REMOTE_K8S_ENDPOINT> with the endpoint of the remote K8s cluster
- Replace <REMOTE_K8S_CA> with the CA of the remote K8s cluster
- Replace <REMOTE_K8S_CERT> with the Certificate of the remote K8s cluster
- Replace <REMOTE_K8S_DATA> with the Key of the remote K8s cluster


## Deploying the app into K8s

Once replaced all the values, just run:

> `kubectl apply -f tsm-cloud-burster.yaml`


## Checking the behavior

Check the logs of the tsm-cloud-burster pod before, during and after the load to check if cloud bursting excess is calculated correctly. Example of these logs before and during cloud bursting moment:

> tsm-cloud-burster No Cloud Bursting! Max replicas 4 / Desired value 1
> 
> tsm-cloud-burster deployment.apps/shopping scaled
> 
> tsm-cloud-burster No Cloud Bursting! Max replicas 4 / Desired value 2
> 
> tsm-cloud-burster deployment.apps/shopping scaled
>
> tsm-cloud-burster  Cloud Bursting! Max replicas 4 / Desired value 4 / Excess on the other cluster=1
>
> tsm-cloud-burster  deployment.apps/shopping scaled
>
> tsm-cloud-burster  Cloud Bursting! Max replicas 4 / Desired value 4 / Excess on the other cluster=2
>
> tsm-cloud-burster  deployment.apps/shopping scaled

