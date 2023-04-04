
# Access-list 20 push for Cisco IOS-XE

## Introduction

This script pushes configuration of ACL20 to a Cisco devices that are listed in an inventory file.
Author: Sergei Ondar
Dependencies:

1. https://dev.azure.com/chevron/TPS-ITC-AnsibleNetwork/_git/config_archive_rollback


## Change steps: 

1. Push current admin password:

        ```
        admin privilege 15 secret HIDDEN_PASSWORD
        ```

1. Push current enable password:

        ``` 
        enable secret HIDDEN_PASSWORD
        ```

1. Push current password of last resort: 

        ```
        line con 0
         password HIDDEN_PASSWORD
        ```

1. Configure rollback command:

        ```
        archive 
         path {{ dir }}
        cofnigre terminal revert timer idle 5
        ```

1. Remove current ACL20 on the device:

        ```
        no access-list 20
        no ip access-list standerd 20
        ```

1. Configure new ACL20 on the device:

        ```
        ip access-list standard 20
         10 permit 139.65.136.0 0.0.3.255
         20 permit 139.65.140.0 0.0.3.255
         30 deny   any log
        ```

1. Reset SSH connection
1. Confirm change:

        ```
        configure confirm
        ```

1. Write running-config to startup-config

        ```
        wr
        ```

## Backup plan

If after configuring new ACL20, a new connection fails, then running-config will be automatically rolled back after 5 minutes.