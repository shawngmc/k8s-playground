# neo-k3os: K3OS on Proxmox
## Initial Setup
1. Upload ISO to server
2. Create VM
3. Install
4. Upload ssh key from github
5. Reboot
6. Login via SSH
7. Set up ```/var/lib/rancher/k3os/config.yaml```
```
hostname: neo-k3os
ssh_authorized_keys:
- "github:shawngmc"
```
8. Reboot
9. Copy the kubectl config from /etc/rancher/k3s/k3s.yaml
 
# wrangler: Lightweight standalone management box w/ Rocky 9.1
## Initial Setup
1. Upload ISO to server
2. Create VM
  - Use the ‘host’ CPU type since Rocky 9.1 needs advanced CPUs
3. Install
4. Reboot
5. Login vis SSH
6. Install kubectl
```
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
sudo yum install -y kubectl
```
7. Install Helm and git
```
sudo yum install git -y
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```
8. Save the contents of the k3s.yaml at ~/.kube/config
9. Replace the 127.0.0.1 with the IP address
## Errata
### Why not use the K8s Dashboard?
It's finicky. By default, it requires a long token, but more importantly it has flaky referrer checking. It CAN be used without kubectl proxy, but it just gets messy.

 

# Refs
- https://betterprogramming.pub/k3s-k3os-kubernetes-docker-containers-installation-setup-cluster-ee9ccfd51a4d
- https://github.com/rancher/k3os/blob/master/README.md
- https://gist.github.com/sebstyle/0a69f8bf18dd8c4aac73f3a25ff30739
- https://www.rancher.com/quick-start
- https://canthonyscott.com/setting-up-a-k3s-kubernetes-cluster-within-proxmox/
https://github.com/k3s-io/klipper-lb
