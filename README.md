# Yet Another OpenShift Install Helper
yaoih is a bash script that automates bringing up an OpenShift 4.x cluster using an installer from the OpenShift
[mirror](https://mirror.openshift.com/pub/openshift-v4/clients/).

```
yaoih (-r <release> | -p <version>) -c <location of cloud specific install-config.yaml> -d <install dir> -n <network config> [-x] <first delete the specified cluster>
```
## -r release
The `openshift-install` [release version](https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/) to be used.
Example: 4.4.4 or latest-4.4

## -p version
The `openshift-install` [preview version](https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp-dev-preview/)
to be used. Example: 4.6.0-0.nightly-2020-06-16-214732 or latest-4.6

Either -r or -p needs to be specified. -r takes precedence over -p.

## -c location of cloud specific install-config.yaml
Specify the `install-config.yaml` that has been generated using the `openshift-install create install-config` command.

## -d install dir
The directory that will be used by `openshift-install`.

## -n network config file
This needs to be used only if you are trying to change network configuration.
An example hybrid OVN Network manifest that will be used by `openshift-install` is show below:
```
apiVersion: operator.openshift.io/v1
kind: Network
metadata:
  creationTimestamp: null
  name: cluster
spec:
  clusterNetwork:
  - cidr: 10.128.0.0/14
    hostPrefix: 23
  externalIP:
    policy: {}
  networkType: OVNKubernetes
  serviceNetwork:
  - 172.30.0.0/16
  defaultNetwork:
    type: OVNKubernetes
    ovnKubernetesConfig:
      hybridOverlayConfig:
        hybridClusterNetwork:
        - cidr: 10.132.0.0/14
          hostPrefix: 23
```

## -x first delete the specified cluster
Optional argument that can be used if you want to first delete the cluster before creating a new one.

## INSTALLER_BIN
This is a a required environment variable that specifies the location where yaoih will download the `openshift-install`
binary
