# Yet Another OpenShift Install Helper
yaoih is a bash script that automates bringing up an Hybrid OVN OpenShift 4.x cluster using an installer from the 
OpenShift [release](https://openshift-release.svc.ci.openshift.org/) page.

```
yaoih (-r <release> | -v <version>) -c <location of cloud specific install-config.yaml> -d <install dir> -n <network config> [-x] <first delete the specified cluster>
```
## -r release
The `openshift-install` release version to be used. Example: 4.5.0-0.ci will result in the latest 4.5 CI installer to be
picked that has been marked green on the release page.

## -v version
The exact `openshift-install` release version to be used. Example: 4.5.0-0.nightly-2020-04-15-172400. 

Either -r or -v needs to be specified. -v takes precedence over -r.

## -c location of cloud specific install-config.yaml
Specify the `install-config.yaml` that has been generated using the `openshift-install create install-config` command.

## -d install dir
The directory that will be used by `openshift-install`.

## -n network config file
This is the hybrid OVN Network manifest that will be used by `openshift-install`. Example:
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
