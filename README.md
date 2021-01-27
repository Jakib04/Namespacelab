# Namespacelab


```bash
ip netns add namespace1
ip netns add namespace2


ip netns exec namespace1 \
        ip address show

# Create the two pairs.
ip link add veth1 type veth peer name br-veth1
ip link add veth2 type veth peer name br-veth2

# Associate the non `br-` side
# with the corresponding namespace
ip link set veth1 netns namespace1
ip link set veth2 netns namespace2

ip netns exec namespace1 \
        ip address show


ip netns exec namespace1 \
        ip addr add 192.168.1.11/24 dev veth1

ip netns exec namespace1 \
        ip address show

ip netns exec namespace2 \
        ip addr add 192.168.1.12/24 dev veth2

ip link add name br1 type bridge
ip link set br1 up
ip link | grep br1



ip link set br-veth1 up
ip link set br-veth2 up

ip netns exec namespace1 \
        ip link set veth1 up
ip netns exec namespace2 \
        ip link set veth2 up

# Add the br-veth* interfaces to the bridge
# by setting the bridge device as their master.
ip link set br-veth1 master br1
ip link set br-veth2 master br1

ping 192.168.1.12

ip netns exec namespace1\
        ip route

ip netns exec namespace1 \
        ping 192.168.1.12


ip netns exec namespace1 \
        ping 8.8.8.8

ip -all netns exec \
        ip route add default via 192.168.1.10

ip netns exec namespace1 \
        ip route


      ip netns exec namespace1 \
        ping 8.8.8.8  


  
iptables \
        -t nat \
        -A POSTROUTING \
        -s 192.168.1.0/24 \
        -j MASQUERADE

   ip netns exec namespace1 ping 8.8.8.8   
```

