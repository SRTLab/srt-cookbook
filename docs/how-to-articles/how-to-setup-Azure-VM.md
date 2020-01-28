# How to Setup Virtual Machine in Azure

1. Create virtual machine specifying parameters listed below
    * Image: Ubuntu Server 18.04 LTS, or alternative
    * Size: Standard D4s v3 (4 vcpus, 16 GiB memory), or alternative
    * Username
    * SSH public key

2. Open UDP ports required for SRT transmission.

Once the virtual machine is created, it's important to setup a networking rule in order to open UDP ports required for SRT transmission. Go to `Settings` &#8594; `Networking` &#8594; `Add inbound port rule` and specify the ports to be opened in `Destination port ranges` field, e.g., 4200.