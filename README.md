# pilosus-ansible

### Gather facts of certain server

Get info about `sordes` server from `hosts` file:

```
ansible sordes -m setup -i hosts -u root
```

### Ping servers

Ping all servers of group `digital_ocean` from the `hosts` file:

```
ansible digital_ocean -i hosts -m ping -u root
```

### Run server init playbook

Play scenario for server `sordes` only:

```
ansible-playbook -l sordes playbook-server-init.yml -i hosts -u root
```
