## Broad BGP topics not specific to any vendor

* [Wikipedia](https://en.wikipedia.org/wiki/Border_Gateway_Protocol)
* [What is BGP?](https://www.cloudflare.com/learning/security/glossary/what-is-bgp/) from Cloudflare blog
* [How BGP Selects the Best Route for Your Packets](https://deploy.equinix.com/blog/bgp-attributes-and-how-bgp-selects-the-best-route/) from Equinix

## BIRD documentation

* [BIRD user guide](https://bird.network.cz/?get_doc&f=bird.html)
* [BGP protocol documentation](https://bird.network.cz/?get_doc&v=20&f=bird-6.html#ss6.4)

## BIRD cheat sheet

* `birdc configure` - reload configuration files
* `birdc show interfaces` - list all network interfaces and their current status
* `birdc show protocols` - show brief information on BGP and other protocols
* `birdc show protocols all` - show BGP and other protocols, more details on every BGP neighbour
* `birdc show protocols all <protocol id>` - show more details on one neighbour only
* `birdc show route` - show routing table
* `birdc show status` - show BIRD status
* `birdc show symbols` - show all defined symbols in the BIRD configuration
* `birdc show route filter <filter id>` - show route filter info
* `birdc show route export <protocol id>` - show exported routes

Extend this list with your own commands!
