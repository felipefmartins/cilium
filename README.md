# VERIFICAR A ULTIMER VERSAO ESTÁVEL
https://github.com/cilium/cilium
https://github.com/cilium/cilium/releases

Version: 1.17.2

```sh
helm repo update
export VERSION=1.17.2
mkdir  v${VERSION}
```

# CREATE MANIFESTS WITH INGRESS
```sh
helm template cilium cilium/cilium \
    --version ${VERSION} \
    --namespace kube-system \
    --set ipam.mode=kubernetes \
    --set kubeProxyReplacement=true \
    --set securityContext.capabilities.ciliumAgent="{CHOWN,KILL,NET_ADMIN,NET_RAW,IPC_LOCK,SYS_ADMIN,SYS_RESOURCE,DAC_OVERRIDE,FOWNER,SETGID,SETUID}" \
    --set securityContext.capabilities.cleanCiliumState="{NET_ADMIN,SYS_ADMIN,SYS_RESOURCE}" \
    --set cgroup.autoMount.enabled=false \
    --set cgroup.hostRoot=/sys/fs/cgroup \
    --set k8sServiceHost=localhost \
    --set hubble.relay.enabled=true \
    --set hubble.ui.enabled=true \
    --set l2announcements.enabled=true \
    --set ingressController.enabled=true \
    --set ingressController.loadbalancerMode=shared \
    --set k8sServicePort=7445 > ./v${VERSION}/cilium.yaml
```

# CREATE MANIFESTS WITHOUT INGRESS
```sh
helm template cilium cilium/cilium \
    --version ${VERSION} \
    --namespace kube-system \
    --set ipam.mode=kubernetes \
    --set kubeProxyReplacement=true \
    --set securityContext.capabilities.ciliumAgent="{CHOWN,KILL,NET_ADMIN,NET_RAW,IPC_LOCK,SYS_ADMIN,SYS_RESOURCE,DAC_OVERRIDE,FOWNER,SETGID,SETUID}" \
    --set securityContext.capabilities.cleanCiliumState="{NET_ADMIN,SYS_ADMIN,SYS_RESOURCE}" \
    --set cgroup.autoMount.enabled=false \
    --set cgroup.hostRoot=/sys/fs/cgroup \
    --set k8sServiceHost=localhost \
    --set hubble.relay.enabled=true \
    --set hubble.ui.enabled=true \
    --set l2announcements.enabled=true \
    --set k8sServicePort=7445 > ./v${VERSION}/cilium-noingress.yaml
```