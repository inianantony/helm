# Helm Installation

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

Helm uses the kubectl Kubernetes context and the namespace.

Configure helm to update its repo by

``` bash
helm repo add stable https://kubernetes-charts.storage.googleapis.com
```