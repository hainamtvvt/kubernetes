# Install K8s Master

```
export INSTALL_K3S_VERSION=v1.25.4+k3s1
curl -sfL https://get.k3s.io | sh -s - --flannel-backend=none --disable-network-policy --disable=traefik --write-kubeconfig-mode 644 --write-kubeconfig ~/.kube/config

NODE_TOKEN= 'cat /var/lib/rancher/k3s/server/node-token' (1)
```

# Install K8s Worker

```
NODE_TOKEN='...' (từ mục (1) )
'

curl -sfL https://get.k3s.io | K3S_URL=https://<ip_master>:6443 K3S_TOKEN=${NODE_TOKEN} sh -
```
