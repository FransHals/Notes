# Notes
Notes for April - Sep
#run as root
$ sudo su
#crete network namespace
$ ip netns add blue
$ ip netns add Hals
#check the ns list
$ ip netns list

#assign VIRTUAL Ethernet interfaces to ns, then configure them for ns
#veth is a bit like tunnel (in pairs, like tube). Create veth pair,
$ ip link add veth0 type veth peer name veth1
$ ip link list
#on the list, could see the pair all belong to default/global ns (connected to physical interfaces)
#move/assign them to my ns
$ ip link set veth1 netns blue
#now, rather than the previous list, find veth1 on blue's list, [execute command in a different ns] follows the actual command
$ ip netns exec blue ip link list

#configure veth1 in the blue ns, [ifconfig]/[ip addr]/[ip link]/[ip route] assign IP addr and bring the intereface up
$ ip netns exec blue ifconfig veth1 10.1.1.1/24 up
#now that only by [ip netns exec blue] could show 10.1.1.0/24 related interfaces and addrs

#use a bridge to connect netns with physical net. e.g. an OVS (Open vSwitch) or linux bridge
$ ip link add name br0 type bridge
#upon creation, one end of the bridge is connected to the protocol stack
