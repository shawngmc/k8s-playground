# Stack
| Section             | Stack          | Tech                                           |
|---------------------|----------------|------------------------------------------------|
| Host System         | Host           | Pentavus: 5600x (6c/12t), 64GB RAM             |
| Host System         | OS             | Proxmox 8.0                                    |
| Host System         | Virtualization | Proxmox VMs (QEMU-based)                       |
| Host System         | Virt Deployment Automation | Cloud-Init                         |
| Master Control Node | OS             | Debian 12                                      |
| Master Control Node | Control Plane  | Rancher v2                                     |
| Master Control Node | VM Spec        | 1 vCPU, 8 GB RAM  (x1 node)                    |
| Cluster Node        | OS             | Debian 12                                      |
| Cluster Node        | K8s Distro     | RKE2                                           |
| Cluster Node        | VM Spec        | 1 vCPU, 8 GB RAM  (x3 nodes)                   |



# Prep the Debian VM Image
Largely based on https://tcude.net/creating-a-vm-template-in-proxmox/
Assumptions
- Using latest [Debian 12 Bookworm QEMU Image](https://cloud.debian.org/images/cloud/bookworm/) - as of last run, was https://cloud.debian.org/images/cloud/bookworm/20230612-1409/debian-12-generic-amd64-20230612-1409.qcow2
- Need to set VM_ID - 105 in the last test run
1. Create a base VM
   - Right-click on Proxmox host, 'Create VM'
   - General
     - Set VM ID (ex. 105)
     - Set Name (ex. debian-12-template)
   - OS
     - Do not use any media
   - System
     - Check 'Qemu Agent'
   - Disks
     - Remove Scsi0
   - CPU
     - 1 socket/1 core, default type
   - Memory
     - 8192 MB
   - Network (no changes)
3. Select VM, Hardware tab > Add CloudInit Drive
   - Choose a storage source
4. Cloud-init tab
   - User: set username
   - Password: set password
   - SSH Public Key: set, currently ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGRkhk2SmZTf6ytjuPoz10Sp4oso6gxP//6VjOebng+J shawngmc@gmail.com
   - IP Config: set to DHCP
5. Open the Proxmox host CLI
6. Download/attach the image file
```
VM_ID=105
IMAGE_URL=https://cloud.debian.org/images/cloud/bookworm/20230612-1409/debian-12-generic-amd64-20230612-1409.qcow2
STORAGE_POOL=PrimaryStorage
# ----------
IMAGE_FILENAME=$(basename $IMAGE_URL)
wget $IMAGE_URL
# Enable GUI Console Access
qm set $VM-IS --serial0 socket --vga serial0
# Resize the disk image to 80G
qemu-img resize $IMAGE_FILENAME 80G
# Import the disk
qm importdisk $VM_ID $IMAGE_FILENAME $STORAGE_POOL
```
7. Adjust the disk
   - Hardware tab > Unused Disk
   - Check Discard to enable thin provisioning
   - Check SSD Emulation to have it treat it like an SSD
8. Set the boot order
   - Options tab > Boot Order
   - Enable the new disk and move it up the list
9. Start the VM and open the console
10. Prep the image
```
# Run updates
sudo apt update && sudo apt upgrade -y
# Install the qemu-agent
sudo apt install qemu-guest-agent
# Enable the agent
sudo systemctl enable qemu-guest-agent
# Reset the machine-id
cat /dev/null > /etc/machine-id
cat /dev/null > /var/lib/dbus/machine-id
# Clean up
cloud-init clean
# Shutdown
shutdown -h now
```
11. Right-click on VM in list > 'Convert to Template'

# Make the Rancher Node
# - Set up the MAC-IP-DNS mapping
# - Provision the VM
# - ???
# - Add Longhorn Storage: 
https://longhorn.io/


# Make/add the Worker Nodes
# - Set up the MAC-IP-DNS mapping
# - Provision the VM
# - Get the install command from Rancher
https://ranchermanager.docs.rancher.com/pages-for-subheaders/launch-kubernetes-with-rancher
# - Add to worker cluster
# - ???


# Add cluster Applications
# - Install MetalLB (external load balancer)
Use Helm chart provided by Bitnami as described at: http://xybernetics.com/techtalk/how-to-install-metallb-on-rancher-kubernetes-cluster/
# - Install Nginx-ingress-controller (internal load-balancing ingress)
https://docs.rancherdesktop.io/how-to-guides/setup-NGINX-Ingress-Controller/
# - Install Istio (service mesh/traffic monitoring/etc.)
https://ranchermanager.docs.rancher.com/pages-for-subheaders/istio-setup-guide
https://ranchermanager.docs.rancher.com/pages-for-subheaders/istio
# - Install Longhorn
https://ranchermanager.docs.rancher.com/integrations-in-rancher/longhorn#installing-longhorn-with-rancher
