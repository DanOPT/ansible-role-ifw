Ansible Role for Icinga for Windows
===================================

A simple role to install and configure Icinga for Windows agents assigned to an unique parent and zone defined in group_vars/<zone-name> and Inventory.

Requirements
------------

Please check https://docs.ansible.com/ansible/latest/user_guide/windows.html

Role Variables
--------------

`ifw_global_zones` Which default Icinga global zones do you want to add?  

[0] Add "director-global" and "global-templates" zones  
[1] Add "director-global" zone  
[2] Add "global-templates" zone  
[3] Add no default global zone  

`ifw_force_certificate_creation` Enforce certificate creation (default: 0):  

[0] Do not enforce certificate creation  
[1] Enforce certificate creation  

`ifw_hostname` How do you want to name your host object (default: 1):  

[0] "i2-agent.domain": FQDN (current)  
[1] "i2-agent.domain": FQDN (lowercase)  
[2] "I2-AGENT.DOMAIN": FQDN (uppercase)  
[3] "i2-agent": Hostname (current)  
[4] "i2-agent": Hostname (lowercase)  
[5] "I2-AGENT": Hostname (uppercase)  
[6] Set custom Hostname

`ifw_port` Port Number for communication with the REST API (default: 5665)

`ifw_connection` How do you want to connect to the parent (default: 0):

[0] Connecting from this system  
[1] Connecting from parent system  
[2] Connecting from both systems  
[3] Icinga Director Self-Service API  

`ifw_certificate_creation` How do you want to create the Icinga certificate (default: 0)

[0] Sign certificate manually on the Icinga CA master
[1] Sign certificate with a ticket
[2] Sign certificate with local ca.crt

`ifw_firewall` Select if your Windows Firewall should be opened for the Icinga port (default: 1)  

[0] Open Windows Firewall for incoming Icinga connections  
[1] Do not open Windows Firewall for incoming Icinga connections  

`ifw_agent_version` Specify the version of the Icinga Agent you want to install (default: 'release')

`ifw_intall_agent` Select if the Icinga Agent should be installed (default: 0)

`ifw_agent_directory`  Path where to install the Icinga Agent into (default: 'C:\\Program Files\\ICINGA2')

`ifw_install_plugins` Specifiy if plugins should be installed (default: 0):  

[0] Install plugins  
[1] Do not install plugins  

`ifw_install_service` Please select of you want to install the Icinga for Windows service:  

[0] Install Icinga for Windows Service  
[1] Do not install Icinga for Windows service  

`ifw_ca_server` Specify a custom CA Server (default: undefined, has to be a string with IP Address or hostname)

`ifw_install_api_checks` Select if you want to enable the Api-Checks feature (default: 1):  

[0] Do not install Api-Checks feature  
[1] Install Api-Checks feature  


Dependencies
------------

No depencies of other roles.

Example Playbook, Inventory and group_vars file
------------------------------

For this role a group_vars file, an inventory, and a Playbook are required.

**Example Playbook:**
```
    - hosts: windows
      roles:
         - ansible-role-ifw
```

**Example group_vars/master (for parent address and node name):**
```
    ifw_parent_nodes:
      - 'i2-master'
      #- 'i2-master-2'
    ifw_parent_address:
      - '10.0.0.100'
      #- '10.0.0.101'
```

Has always to be a list, even if only one parent will be assigned. First node name in `ifw_parent_nodes` belongs to the first address in `ifw_parent_address`.

There can be more than one zone specified in group_vars/.

Filename has to be the group name and will be used as the name of the parent zone.

**Example Inventory:**
```
    [master]
    i2-agent pki_ticket=<pki_ticket>
```
**Every host can only be part of one group** that will define the parent node names and address. The group name will also be used to define the zone name.

There can also be more than one group as a zone specified in the Inventory.
    
License
-------

MIT

Author Information
------------------

Daniel Patrick - daniel.patrick@netways.de
