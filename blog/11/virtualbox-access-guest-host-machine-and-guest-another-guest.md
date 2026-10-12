

# Virtualbox - Access Guest from Host machine and Guest from another Guest

This is really simple. Open the settings for the VM you want to perform the magic on. Set the network adapter to be "Bridged adapter" and choose the card. If you choose eth0, for example, then it will be like your virtual machine is plugged into the eth0 network the same way the host machine is. Machines on the same network can also ping your virtual machine.
