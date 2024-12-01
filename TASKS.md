## General instructions

#### You can always restart Containerlab, if something feels wrong

```
sudo containerlab destroy
sudo containerlab deploy
```

#### Use power of Git!

Use branches of `master` to switch between different tasks. Don't forget to reload BIRD configuration after doing so!

#### You should not touch anything in topology

You likely shouldn't delve into the details of `bgp-lab.clab.yaml` or modify anything there. If something seems wrong in this file, it is likely intentional and part of the task. It's best to act as if this file is not there at all.

#### Avoid easy ways

You are encouraged to rely on your Linux CLI expertise for configuration and troubleshooting. Refrain from looking into `bgp-lab.clab.yaml` or BIRD configuration files unless instructed by the task.

It might be tempting to delve into the scripts to see what's executed when the container starts, but doing so won't enrich your learning experience. Particularly for troubleshooting tasks — discovering the issue beforehand removes the challenge and satisfaction of diagnosing it yourself. Instead, make use of your knowledge of `ip`, `ss`, `nc`, `traceroute`, `iptables`, `sysctl`, `birdc` and so on.

#### Bird config files vs show commands

As much as possible, try using show commands, such as running `birdc show protocols` instead of looking into the configuration file to see which BGP neighbors are configured.

#### Topology diagram is your best friend

Keep looking at [topology diagram](topology.pdf) when configuring or troubleshooting anything.

#### Read documentation

Some [documentation](DOCS.md) is provided within this repository, while additional resources are available online. Look for them!

#### Understand the differences in loopback behavior between IPv6 and IPv4

If you assign an IPv4 address like `10.20.30.0/24` on a loopback interface, you can ping any IP within that range and expect a response. This behavior is due to how IPv4 addresses are handled, allowing easier communication across an entire subnet. However, IPv6 operates differently and adheres to stricter routing and security principles. Therefore, if you assign an IPv6 address such as `fc00:123:123::123/48` on a loopback, you cannot successfully ping a different address like `fc00:123:123::321` within that range. IPv6 often requires that you ping the specific assigned address, in this case `fc00:123:123::123`, to receive a response. Remember this distinction when testing end-to-end connectivity in this lab.

#### Other tips

1. Document your process
2. Think of your own tasks and scenarios, this lab is amazing to play with!
3. This is YOUR lab - if something like `tcpdump` or `mtr` is missing in the containers, feel free to install it
4. Master branch has fully working topology - all servers and routers can ping each other, all links are up, all BGP sessions are up. If something is wrong - probably Containerlab has failed to bring something up - check `sudo containerlab deploy` output.
5. `curl -s` any IP address to get the hostname of the responder. Don't forget square brackets for IPv6.

## Theory questions

You are encouraged to research and provide answers to these questions. Some of them may not have a single correct answer, while others might offer multiple solutions. The purpose of this exercise is to enhance your design thinking skills.

1. What are the advantages of using different AS numbers for servers and network infrastructure within our points of presence (POPs)?
2. What mechanisms are available to influence the preference of our POPs within the BGP transit network?
3. In IP communication, protocols like TCP and UDP operate on the concept of flows, where a flow is defined as a unidirectional stream of packets from one host to another. Why is it crucial for all packets within a flow to follow the same path? Moreover, why might the path from server A to server B differ from the path from B to A?

## Configuration tasks

You might want to work in different branches, based on `master`. Order of tasks is not important.

### Task 1

Your peering partners have raised concerns about the excessive number of prefixes you are announcing. Make sure not to advertise any prefixes longer than `/24` for IPv4 and `/48` for IPv6 from both POP locations. It is important that connectivity remains unaffected. You are only permitted to modify the configuration on `router01.lab01` and `router01.lab02`.

### Task 2

Security team has highlighted a concern regarding BGP advertisements containing private autonomous system numbers `65001` and `65002` in the AS PATH. Ensure these ASNs are excluded from the AS PATH for all advertised routes. You are only permitted to update the configurations on `router01.lab01` and `router01.lab02`.

### Task 3

Requests from customers of AS 102 are currently distributed between two POPs. The planning team desires that POP #2 handles all requests to the network `fc00:910b:1/48` from AS 102. Furthermore, ensure that requests from AS 101 to this network continue to be partially served by POP #1. You are not allowed to touch configuration of transit network.

### Task 4

You are a network engineer in AS 102. Your goal is to route all traffic destined for `10.200.2.0/24` through your peering partner AS 103 by preference. In the event that the connection to AS 103 is unavailable, AS 101 should serve as the backup route. You are only allowed to modify the configuration of devices within AS 102.

### Task 5

Your POP #2 is experiencing high traffic. Configure `server03.lab02` to handle all incoming traffic directed toward the anycast range `10.202.3.0/24` originating from outside this POP. You are only allowed to modify the configuration of `server03.lab02`.

### Task 6

Security team has raised concerns regarding the security of certain services within the POP #1 colo-level anycast network, specifically the range `10.201.1.0/24`. Ensure that this network is inaccessible from outside the POP while maintaining internal connectivity. Note that creating new firewall rules is not permitted.

## Troubleshooting tasks

**You need to switch to `tshoot` branch**:
```
sudo containerlab destroy
git checkout tshoot
sudo containerlab deploy
```

After this, **DO NOT** look into `bgp-lab.clab.yaml` or BIRD configuration files, you will spoil all fun. After you figure out the issue, fix it using CLI commands or by updating BIRD configuration files. Do not try to find issues by comparing BIRD configs with each other. Use BIRD and Linux CLI commands as much as possible. Issues are not necessarily in BIRD configuration. Order of tasks is not important.

### Task 1

Investigate and resolve the issue preventing `server03.lab02` from handling anycast requests.

### Task 2

Customers in AS 102 are experiencing issues accessing the web service at the IPv4 address `10.200.1.5`. However, the service functions correctly with the IPv6 address `fc00:910b:1::`. Diagnose and resolve the problem. Use `ping` from `eyeball01.tra02` to verify.

### Task 3

Requests to the colo-level anycast address `10.201.3.199` from `router01.lab01` are consistently directed to the same server, leading to overload. Identify the cause of this issue and resolve it.

### Task 4

`server02.lab01` never gets any anycast traffic for address `fc00:910b:2::`. Identify the cause of this issue and resolve it.

### Task 5

Identify and resolve the issue preventing `server01.lab01` from handling any anycast requests.

### Task 6

`server02.lab02` is unable to access the colo-level anycast service at the IP address `fc00:c001:2:1::`. Investigate the cause and resolve the issue.

### Task 7

Requests from customers of AS 101 to the anycast address `10.200.2.2` are consistently routing through POP #2 via AS 102, despite the proximity of POP #1. Investigate and resolve this issue.

### Task 8

Requests from customers of AS 103 to the anycast address `fc00:910b:1::` are consistently routing through POP #1 via AS 102, despite the proximity of POP #2. Investigate and resolve this issue.
