# Network HostVar Creator

Example Ansible playbook to parse brownfield network devices and return Infrastructure as Code (IaC). This allows network teams to take their existing configurations (BGP, VLANs, etc) and store them as variables. With this approach we are now able to use [Network Resource Modules](https://docs.ansible.com/ansible/latest/network/user_guide/network_resource_modules.html) to perform declarative configurations on our devices and use our variable as our source of truth.

## Usage

This is intended to be used for demonstration purposes or for initial parsing of a devices current state. Once this baseline is established the goal is to start using the "bgp_config" variable that is created in host_vars/{{  device }}.yml to make changes to the devices BGP state.

The only configuration this parses is BGP but there are resource modules for VLANs, interfaces, OSPF, logging and many others that we can take this approach with. 

The best part about the resource modules? By changing the modules from "arista.eos.eos_bgp_global" to something like "cisco.nxos.nxos_bgp_global" or "junipernetworks.junos.junos_bgp_global" we can take the same approach with managing multiple platforms. 

<img src="https://i.pinimg.com/originals/ca/cf/9f/cacf9f86392afeac7d83fdd2291b53af.jpg" width="300" height="300">



## Example Output

Hostvar file created under host_vars/{{ device }}.yml

```yaml
### BGP

bgp_config: |
  router bgp 64512
   neighbor 10.0.1.1 remote-as 64513
   neighbor 10.0.1.1 description Leaf1
   neighbor 10.0.1.1 maximum-routes 12000
   neighbor 10.0.1.3 remote-as 64513
   neighbor 10.0.1.3 description Leaf2
   neighbor 10.0.1.3 maximum-routes 12000
   neighbor 10.0.1.5 remote-as 64514
   neighbor 10.0.1.5 description Leaf3
   neighbor 10.0.1.5 maximum-routes 12000
   network 10.0.0.0/24
   network 10.0.1.0/24
   network 10.0.2.0/24
   router-id 10.0.250.1
```
