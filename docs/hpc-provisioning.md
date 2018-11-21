# Provisioning

We will be talking about [**provisioning**](https://www.techopedia.com/definition/4069/provisioning-computing-computing) in the computing sense and mostly related to just high performance computing. Although this can go pretty far as HPC spans most of computing.

Definition:

> Bring some resource online for use in the HPC Cluster such as:
>
> - Nodes
> - Users
> - Virtual Machines
> - Storage

## Outline

- Nodes Bare Metal
    - DNS
    - PXE
    - TFTP
    - IPMI
    - ILO
    - BIOS
    - BOOT Order
    - Provisioning Systems
        - ROCKS
        - OpenHPC
        - Cobbler
- Users
    - LDAP
    - Kerberos
    - [NIS](https://en.wikipedia.org/wiki/Network_Information_Service)
    - Local Accounts
- Virtual Machines
    - Cloud
        - Amazon Web Services (AWS)
        - Google Cloud Platform (GCP)
        - MS Azure
    - HyperVisor Stacks
        - ZenServer
        - Proxmox
        - kvm/libvirt
        - VmWare
        - MS HyperV