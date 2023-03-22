# Yet Another OpenShift Install Helper
yaoih is a bash script that automates bringing up an OpenShift 4.x cluster using an installer from the OpenShift
[mirror](https://mirror.openshift.com/pub/openshift-v4/clients/).

```
yaoih (-r <release> | -p <version>) -c <location of cloud specific install-config.yaml> -d <install dir> -n <network config> [-x] <first delete the specified cluster>
```
## -r release
The `openshift-install` [release version](https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/) to be used.
Example: `4.4.4` or `latest-4.11`

## -p version
The `openshift-install` [CI](https://openshift-release.apps.ci.l2s4.p1.openshiftapps.com/) stream to be used. This will
pick the latest version in that stream. Example: `4.14.0-0.ci`

## -v version
The `openshift-install` [CI](https://openshift-release.apps.ci.l2s4.p1.openshiftapps.com/) version to be used.
Example: `4.14.0-0.ci-2023-03-20-081138`

Either -r or -p or -v needs to be specified or the existing binary in [$INSTALLER_BIN](#INSTALLER_BIN) will be used. If
neither -r or -p or -v is specified and there is no existing binary, an error will be thrown. The -r takes precedence
over -p or -v. -v takes precedence over -p.

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
