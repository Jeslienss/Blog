Setup VMware Bridge Network
===================================

Normally, VMware will NAT in network setting. It will keep the same configuration of the HOST. However, sometimes we want our VM has a different network configuration from HOST. Here, we need to use Bridge mode.

**Virtual Machine Settings**
-----------------------------------
In network adapter, set network connection to be *Bridge*.

**VM OS settings**
-----------------------------------
1. You need to figure out the following information of HOST