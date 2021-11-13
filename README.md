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

### Encrypt / descrypt vault values

See the [docs](https://docs.ansible.com/ansible/latest/user_guide/vault.html)

```
# encrypt
ansible-vault encrypt_string --vault-password-file ~/.ansible/.sordes_pass.txt 'String to encrypt' --name 'variable_name'

# decrypt specific variable
ansible localhost -m ansible.builtin.debug -a var="env_slack_token" -e "@vars/env.yml" --vault-password-file ~/.ansible/.sordes_pass.txt
```


### Run service init playbook

```
# Run all tasks
ansible-playbook --limit sordes playbook-app-init.yml --inventory hosts --user root --vault-password-file ~/.ansible/.sordes_pass.txt

# Run tasks with selected tags
ansible-playbook -l sordes playbook-app-init.yml -i hosts -u root --vault-password-file ~/.ansible/.sordes_pass.txt --tags "env"
```


### Debugging service

```
# See service status
systemct status dienstplan.service
systemct is-active dienstplan.service

# See logs
journalctl -u dienstplan.service
```
