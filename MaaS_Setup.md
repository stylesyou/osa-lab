## MaaS Setup for OSA Deployment

### Network Planning

```sh
Network :

PXE/Mgmt  :br-mgmt      : 192.168.100.0/24
Storage   :br-storage   : 192.168.101.0/24
Tunnel    :br-vxlan     : 192.168.102.0/24
Ext1      :br-Ext1      : 192.168.103.0/24
Ext2     : br-ext2      : 192.168.104.0/24
```

The following table shows bridges that are to be configured on hosts.

Bridge name	Best configured on	With a static IP
br-mgmt	On every node	Always
br-storage	On every storage node	When component is deployed on metal
On every compute node	Always
br-vxlan	On every network node	When component is deployed on metal
On every compute node	Always
br-vlan	On every network node	Never
On every compute node	Never

### WIP ###
