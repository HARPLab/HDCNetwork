# HDCNetwork

Human (Pose+Position) Data Collection effort 2018


## Node Synchronization
Currently, using Chrony for NTP-based sync between nodes in an [isolated network setting](https://chrony.tuxfamily.org/manual.html#Isolated-networks) For more detail see [here](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/sect-setting_up_chrony_for_different_environments#sect-Setting_up_chrony_for_a_system_in_an_isolated_network)

Under this setting, one node is set as central time-server and the other nodes are clients of this. They sync up with this computer. This lends itself to an identical config file for all nodes and a single different one for the central node. 

*Also, The UDP port number 123 needs to be open in the firewall in order to allow the client access*



#### Central timeserver config
Example in `chrony_config/central.conf`. Do `sudo cp chrony_config/central.conf /etc/chrony/chrony.conf`
Static DHCP allocation of master based on MAC addresses is probably a good idea.

#### Client config

In the client config file (`chrony_config/config.conf`), `master` needs to be replaced by the IP of the central node.
Send it off to all the `client`s with:

```
scp chrony_config/client.conf client:/tmp/chrony.conf 
ssh client sudo mv /tmp/chrony.conf /etc/chrony/chrony.conf
```

This is 2 step because you need sudo on the remote client. 


##### Other notes

For `chronyc` to accept remote commands over ssh from the IP address `IPadd`, add a `cmdallow IPadd` in the client files.

#### TODOS
With set clients:
[ ] add all client IPs to ~.ssh/config on central node
[ ] make the chrony config copy a bash script & iter over hostnames


