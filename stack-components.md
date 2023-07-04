# Host Type
| Type                | Prospects | Notes                                                         |
|---------------------|-----------|---------------------------------------------------------------|
| Commodity PC Server | High      | Pentavus: 5600x, 64GB RAM                                     |
| Mini PC Cluister    | NO        | Cost is a little to high for decent performance at the moment |

# Host OS
| Type                      | Prospects | Notes                                                                                                                                                                                                                                                                        |
|---------------------------|-----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Proxmox                   | High      | - Already installed on Pentavus <br>- Without EU$105/year license, updates are painful <br>- Does not yet handle heterogenous cores (chiplet, BIGLittle, P/E-core) well: https://forum.proxmox.com/threads/hey-proxmox-community-lets-talk-about-resources-isolation.124256/ |
| Harvester                 | NO        | - VERY High system requirements: https://docs.harvesterhci.io/v1.1/install/requirements                                                                                                                                                                                      |
| OpenStack                 | NO        | - Seems far more complicated                                                                                                                                                                                                                                                 |
| Debian/Ubuntu as LXD Host | Moderate  | - LXD UI seems pretty slick <br>- Much lighter weight than full VMs <br>- More manual maintenance                                                                                                                                                                            |

# Host Virtualization
| Type    | Prospects | Notes                                                                                              |
|---------|-----------|----------------------------------------------------------------------------------------------------|
| LXC     | Moderate  | - Seems slightly more complex than Docker containers <br>- High performance/efficiency             |
| LXD     | Moderate  | - Seems much more integrated than LXC alone <br>- Not on Proxmox <br>- High performance/efficiency |
| QEMU VM | High      | - Lower learning curve <br>- Moderate performance/efficiency                                       |

# Deployment Automation
- Terraform
- Ansible
- Rancher (RKE2/K3s only)
- Kubespray
  - NO: Uses Ansible
  - NO: Doesn't help with management at all
- Cloud-init
- Ignition/Butane (Flatcar only)
  - Custom file formats out the wazoo
  - Need to figure out a way to make them accessible

# Kubernetes Distribution
- RKE2 (https://docs.rke2.io/)
  - More conformant than K3s
  - Better security than K3s
  - Includes NGINX Ingress Controller OOTB
- K3s (https://docs.k3s.io/)
  - Includes Traefik Ingress Controller OOTB, but can disable via --disable traefik
  - Includes Klipper single-node LB OOTB, but can disable via --disable servicelb
- RKT - NO, EOL - https://github.com/rkt/rkt/issues/4024
- RKE1 (https://rke.docs.rancher.com/) - NO - Not EOL, but largely redundant

# Kubernetes Control Plane

# Guest OS
Note: This should be easy to change. I should play around.
| Type               | Prospects | Notes                                                                                                                                                                                    |
|--------------------|-----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Flatcar Linux      | Low       | - Moderate deployment complexity - Ignition/Butane configs, etc. <br>- QEMU Image available <br>- More secure <br>- Even the LTS is only 18 months/6 month overlap <br>- Kernel 5.15.113 |
| Debian             | High      | - Low deployment complexity <br>- Recent release train - More stable than Ubuntu for this purpose <br>- Kernel 6.1                                                                       |
| Rocky 9            | Moderate  | - Low deployment complexity <br>- Future is kind of uncertain <br>- Kernel 5.14                                                                                                          |
| Ubuntu LTS 22.04.2 | Moderate  | - Low deployment complexity <br>- Almost as stable as Debian, but getting long in the tooth <br>- Kernel 5.19                                                                            |

# Load Balancer
- MetalLB (https://metallb.universe.tf/)
  - Modes
    - L2
    - BGP 
      - NO, not officially supported by Unifi in the mode we need
- OpenELB
- Klipper (https://github.com/k3s-io/klipper-lb)
  - NO, only works per-node, not cross-node

# Ingress Controller
Ingress Controller
- Traefik Ingress Controller
- NGINX Ingress Controller (https://kubernetes.github.io/ingress-nginx/)


# Storage
- NFS
- Longhorn
- Ceph
