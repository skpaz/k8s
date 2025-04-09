# Tools

## OpenShift CLI (oc)

[The OpenShift CLI](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/cli_tools/openshift-cli-oc) ( `oc` ) is OCP's version of `kubectl`. It offers full support
 for OCP resources and some OCP-specific features.

`oc` can be downloaded via **( ? ) > Command Line Tools** in the upper-right corner of the OCP web console, or from the [Red Hat Customer Portal](https://access.redhat.com/downloads/content/290/ver=4.18/rhel---9/4.18.6/x86_64/product-software). This example downloads `oc` from the OSL cluster.

### Download OC

```bash
curl -L --insecure --output ~/oc.tar \
  "https://downloads-openshift-console.apps-crc.testing/amd64/linux/oc.tar"
```

### Extract OC

```bash
tar -xvf ~/oc.tar \
  && sudo rm -rf ~/oc.tar \
  && sudo mv ~/oc /usr/local/bin/.
```

Validation:

```bash
$ which oc
/usr/local/bin/oc

$ oc version
Client Version: 4.18.0-202502040032.p0.ga50d4c0.assembly.stream.el9-a50d4c0
Kustomize Version: v5.4.2
Server Version: 4.18.1
Kubernetes Version: v1.31.5
```

## kubectl

`kubectl` can also be used with OCP, but lacks support for certain OCP
 resources. Follow the instructions on [Kubernetes' website](https://kubernetes.io/docs/tasks/tools/).

## Helm

A link to download `helm` can be found under **( ? ) > Command Line Tools** in
 the upper-right corner of the Console UI, or you can follow the instructions
 on [Helm's website](https://helm.sh/). Helm's official install methods are via
 Homebrew (macOS/Linux), Chocolatey (Windows), Scoop (Windows), and Snap (Linux).

This example uses the binary provided by Red Hat.

### Download Helm

```bash
curl -L --output ~/helm-linux-amd64.tar.gz \
  "https://developers.redhat.com/content-gateway/file/pub/openshift-v4/clients/helm/3.15.4/helm-linux-amd64.tar.gz"
```

### Extract Helm

```bash
tar -xvf ~/helm-linux-amd64.tar.gz \
  && sudo rm -rf ~/helm-linux-amd64.tar.gz \
  && sudo mv ~/helm-linux-amd64 /usr/local/bin/helm
```

Validation:

```bash
$ which helm
/usr/local/bin/helm

$ helm version
version.BuildInfo{Version:"v3.15.4+60.el9", GitCommit:"fa384522f2878321c8b6b1a06f8ff5f86f47a937", GitTreeState:"clean", GoVersion:"go1.22.7 (Red Hat 1.22.7-2.el9_5)"}
```
