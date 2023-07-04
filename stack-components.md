# Host Type
| Type                | Prospects | Notes                                                         |
|---------------------|-----------|---------------------------------------------------------------|
| Commodity PC Server | High      | Pentavus: 5600x, 64GB RAM                                     |
| Mini PC Cluister    | NO        | Cost is a little to high for decent performance at the moment |

# Host OS
| Type                      | Prospects | Notes  |
|---------------------------|-----------|--------|
| Proxmox                   | High      | - Already installed on Pentavus <br>- Without EU$105/year license, updates are painful <br>- Does not yet handle heterogenous cores (chiplet, BIGLittle, P/E-core) well: https://forum.proxmox.com/threads/hey-proxmox-community-lets-talk-about-resources-isolation.124256/ |
| Harvester                 | NO        | - VERY High system requirements: https://docs.harvesterhci.io/v1.1/install/requirements |
| OpenStack                 | NO        | - Seems far more complicated |
| Debian/Ubuntu as LXD Host | Moderate  | - LXD UI seems pretty slick <br>- Much lighter weight than full VMs <br>- More manual maintenance |

# Host Virtualization
| Type    | Prospects | Notes                                                                                              |
|---------|-----------|----------------------------------------------------------------------------------------------------|
| LXC     | Moderate  | - Seems slightly more complex than Docker containers <br>- High performance/efficiency             |
| LXD     | Moderate  | - Seems much more integrated than LXC alone <br>- Not on Proxmox <br>- High performance/efficiency |
| QEMU VM | High      | - Lower learning curve <br>- Moderate performance/efficiency                                       |

# Deployment Automation
| Type            | Prospects | Notes                                                                                                   |
|-----------------|-----------|---------------------------------------------------------------------------------------------------------|
| Terraform       | High      | - Has a TON of interfacing plugins                                                                      |
| Ansible         | NO        | - Red Hat is not likely to be Homelab-friendly                                                          |
| Rancher         | High      | - Acts as a control plane<br>- Makes RKE2 and K3s very easy to setup                                    |
| Kubespray       | NO        | - Uses Ansible<br>- Doesn't help with management later                                                  |
| Cloud-init      | High      |                                                                                                         |
| Ignition/Butane | Low       | - Flatcar Linux only<br>- 2 custom file formats<br>- Need to figure out a place to host/make accessible |

# Kubernetes Distribution
| Type                                  | Prospects | Notes                                                                                                                                                                |
|---------------------------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [RKE2](https://docs.rke2.io/)         | High      | - More conformant than K3s<br>- Better security than K3s<br>- Includes NGINX Ingress Controller OOTB                                                                 |
| [K3s](https://docs.k3s.io/)           | Moderate  | - Includes Traefik Ingress Controller OOTB, but can disable via --disable traefik<br>- Includes Klipper single-node LB OOTB, but can disable via --disable servicelb |
| RKT                                   | NO        | - EOL - https://github.com/rkt/rkt/issues/4024                                                                                                                       |
| [RKE1](https://rke.docs.rancher.com/) | NO        | - Not EOL, but largely redundant       

# Kubernetes Control Plane
| Type    | Prospects | Notes                                                                      |
|---------|-----------|----------------------------------------------------------------------------|
| Rancher | High      | - Doesn't have to be clustered<br>- Easy way to make RKE2 and K3s Clusters |

# Guest OS
Note: This should actually be easy to change. I should play around.
| Type               | Prospects | Notes                                                                                                                                                                                    |
|--------------------|-----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Flatcar Linux      | Low       | - Moderate deployment complexity - Ignition/Butane configs, etc. <br>- QEMU Image available <br>- More secure <br>- Even the LTS is only 18 months/6 month overlap <br>- Kernel 5.15.113 |
| Debian             | High      | - Low deployment complexity <br>- Recent release train - More stable than Ubuntu for this purpose <br>- Kernel 6.1 |
| Rocky 9            | Moderate  | - Low deployment complexity <br>- Future is kind of uncertain <br>- Kernel 5.14 |
| Ubuntu LTS 22.04.2 | Moderate  | - Low deployment complexity <br>- Almost as stable as Debian, but getting long in the tooth <br>- Kernel 5.19 |

# Load Balancer
| Type                                            | Prospects | Notes  |
|-------------------------------------------------|-----------|--------|
| [MetalLB](https://metallb.universe.tf/)         | High      | - Use L2 mode, BGP is not officially supported by Unifi |
| OpenELB                                         | Moderate  |  |
| [Klipper](https://github.com/k3s-io/klipper-lb) | NO        | - Bundled with k3s - Single node only - not cross-node  |

# Ingress Controller
| Type                                                                    | Prospects | Notes                 |
|-------------------------------------------------------------------------|-----------|-----------------------|
| Traefik Ingress Controller                                              | Low       | - Only default in K3s |
| [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/) | High      | - Industry standard   |


# Storage
| Type                            | Prospects | Notes                                         |
|---------------------------------|-----------|-----------------------------------------------|
| NFS                             | High      | - Already on the NAS                          |
| [Longhorn](https://longhorn.io) | High      | - Seems very simple                           |
| Ceph                            | Low       | - Proxmox includes it<br>- Tends to be fiddly |

# Service Mesh
| Type    | Prospects | Notes                                                                      |
|---------|-----------|----------------------------------------------------------------------------|
| Istio   | High      |  |
