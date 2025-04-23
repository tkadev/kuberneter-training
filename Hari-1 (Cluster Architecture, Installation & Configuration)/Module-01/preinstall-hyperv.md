# Pre install HyperV VMWare

Source: https://medium.com/@enrique.torresds/setting-up-a-kubernetes-cluster-using-ubuntu-virtual-machines-and-hyper-v-part-i-8719f79d045e

### Download Ubuntu ISO, choose Server Install
```
https://releases.ubuntu.com/22.04/
```

### Enable Routing Internal Subnet HyperV, via PowerShell as Administrator
```
Set-ItemProperty -Path HKLM:\system\CurrentControlSet\services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```

### Open Hyper-V manager and configure the settings to store the Virtual Machine configuration files and Virtual Hard Disks.

### In Hyper-V Manager, use the Virtual Switch Manager to create an Internal Network
Assign and address to the virtual adapter created at the Hyper-V host that you can find at Control Panel\All Control Panel Items\Network Connections.
This address will serve as Default Gateway for the Kubernetes nodes.

```
Internal Switch name: vS_k8sCluster 
subnet 192.168.5.0/24
Set IP Default Gateway: 192.168.5.1
```

### Configure a NAT Network via Power Shell (as administrator), enabling the k8s subnet to access Internet.
```
New-NetNAT -Name “ natK8sCluster” -InternalIPInterfaceAddressPrefix 192.168.5.0/24
```

Set Up HyperV VM

### Select Generation 2 VM
### Deselect Dynamic Memory
### Assign RAM 2048 MiB minimal, recommended 4096 MiB
### Choose Internal vSwitch (vs_k8sCluster)
### Create the virtual hard disk (.vhdx) and give it a size of 20 GiB
### Search and use the ubuntu ISO, Boot VM
### If Boot VM failed, disable Secure Boot
### Set IP (for master node)
```
Subnet 192.168.5.0/24
IP: 192.168.5.10
Default Gateway: 192.168.5.1
```
