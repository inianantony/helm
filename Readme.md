# Helm

## Installation

Download the binary from <https://helm.sh/docs/intro/install/> & <https://github.com/helm/helm/releases>
Choose Helm 3 to download, extract it and move the binary to the path

``` bash
sudo mv ~/helm /usr/local/bin/helm
```

Verify the installation by

``` bash
helm version --short

helm env
```

Helm uses the kubectl's Kubernetes context and the namespace.

Configure helm to update its repo by

``` bash
helm repo add stable https://kubernetes-charts.storage.googleapis.com
```

Since we need to access our application through ingress, we need to enable the ingress in minikube

``` bash
minikube addons enable ingress
```

## First Helm Chart

Since our environment is ready, we can create new helm chart by creating a folder `chart`. Inside the folder create a folder called `guestbook` and this `guestbook` will be the name of the helm chart. Inside the `guestbook` folder, create a `Chart.yaml` file with the following lines

### Folder Structure

The folder that the `Chart.yaml` resides is the chart name for helm. In our case `guestbook`. This folder contaies `Chart.yaml` file describing the metadata & properties of chart.

```
-- ChartName (folder)
   |
   |-- Metadata ( Chart.yaml)
   |
   |-- templates ( Kubernetes objects files ie the yaml files)
       |
       |-- service.yaml
       |
       |-- deployment.yaml
       |
       |-- _helpers.tpl ( some helper functions for placeholder values )   
   |
   |-- values.yaml ( values that replaces the placeholder )
```

``` yaml
apiVersion: v2 # helm api version, Helm 3 is v2
type: application
name: guestbook
appVersion: "1.0" # applications version, so if the app has changes this version number changes and helm install will update the app
version: 0.1.0 # chart version to track change in this chart file
description: a helm chart for guestbook 1.0
```

Also inside the `guestbook` folder , create a sub folder `templates` and add the kubernetes yaml files for the frontend app.

Once you have the, then install the app, by

``` bash
helm install demo-guestbook guestbook
```

In the above command `demo-guestbook` is the name of the deployment and the `guestbook` is the name if the helm chart which is the folder location where helm can find the `Chart.yaml` and the `templates`

``` bash
minikube ip
```

will get us the ip of minikube and we can bind it with `frontend.minikube.local` and `backend.minikube.local` in the hosts file for us to access the application from the browser <http://frontend.minikube.local>

we can use

``` bash
helm list --short

helm get manifest demo-guestbook | less

```

to see the deployments by helm

There is a new change made to the application and a new docker image 1.1 is released, then update the `frontend.yaml` file to the new version and also the `Chart.yaml` file on the appVersion part. After making the changes run

``` bash
helm upgrade demo-guestbook guestbook
```

Running

``` bash
helm status demo-guestbook
```

 will show the revision number of the current release. If we need to rollback to previous version because of whatever reason then

 ``` bash
helm rollback demo-guestbook 1
 ```

will rollback the app to the revision 1

Running

``` bash
helm history demo-guestbook
```

can reveal the information about the change history for demo-guestbook release.

``` bash
helm uninstall demo-guestbook
```

will uninstall the release from the cluster with all the kubernetes objects and helm secrets in the cluster.
