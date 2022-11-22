Ansible Role for Icinga for Windows
===================================

A simple role to install and configure Icinga for Windows agents assigned to a unique parent defined in group_vars/<zone-name> and Inventory.

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

[0] "dpatrick-windo": FQDN (current)  
[1] "dpatrick-windo": FQDN (lowercase)  
[2] "DPATRICK-WINDO": FQDN (uppercase)  
[3] "dpatrick-windo": Hostname (current)  
[4] "dpatrick-windo": Hostname (lowercase)  
[5] "DPATRICK-WINDO": Hostname (uppercase)  
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

`ifw_parent_nodes` Parent node names (default: not defined, required and has to be a list, even if only one parent is defined.) E.g.:
```
ifw_parent_nodes:
  - 'dpatrick-i2-master'
```

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

**Example Inventory:**
```
    [master]
    i2-satellite ansible_host=10.0.0.110 pki_ticket=<pki_ticket>  self_service_key=<self_service_key>

    [master:vars]
    ansible_user=<remote_user>
    ansible_password=<password>
    ansible_port=5985
    ansible_connection=winrm
    ansible_winrm_transport=basic
    ansible_winrm_server_cert_validation=ignore
```
**Every host can only be part of one group** that will define the parent node names and address. The group name will also be used to define the zone name.

License
-------

MIT

Author Information
------------------

Daniel Patrick - daniel.patrick@netways.de