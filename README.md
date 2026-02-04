# GET LAST STABLE VERSION
https://github.com/cilium/cilium

https://github.com/cilium/cilium/releases

# UPDATE HELM CHART AND CREATE DIRECTORY
```sh
helm repo update
export VERSION=1.18.6
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
    --set k8sServicePort=7445 \
    --set bpf.masquerade=true \
    --set cni.exclusive=false > ./v${VERSION}/cilium.yaml
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
    --set k8sServicePort=7445 \
    --set gatewayAPI.enabled=true \
    --set gatewayAPI.appProtocol=true \
    --set bpf.masquerade=true \
    --set envoyConfig.enabled=true \
    --set cni.exclusive=false > ./v${VERSION}/cilium-noingress.yaml
```

# CREATE MANIFESTS WITHOUT INGRESS
```sh
helm template cilium cilium/cilium \
    --version ${VERSION} \
    --namespace kube-system \
    --set ipam.mode=kubernetes \
    --set kubeProxyReplacement=true \
    --set socketLB.hostNamespaceOnly=true \
    --set cni.exclusive=false \
    --set gatewayAPI.enabled=false \
    --set securityContext.capabilities.ciliumAgent="{CHOWN,KILL,NET_ADMIN,NET_RAW,IPC_LOCK,SYS_ADMIN,SYS_RESOURCE,DAC_OVERRIDE,FOWNER,SETGID,SETUID}" \
    --set securityContext.capabilities.cleanCiliumState="{NET_ADMIN,SYS_ADMIN,SYS_RESOURCE}" \
    --set cgroup.autoMount.enabled=false \
    --set cgroup.hostRoot=/sys/fs/cgroup \
    --set k8sServiceHost=localhost \
    --set k8sServicePort=7445 \
    --set hubble.relay.enabled=true \
    --set hubble.ui.enabled=true \
    --set l2announcements.enabled=true \
    --set bpf.masquerade=true \
    --set envoyConfig.enabled=true > ./v${VERSION}/cilium-istio-compatible.yaml
```