# dienstplan-deploy

A collection of ansible playbooks for the [dienstplan slack bot](https://github.com/pilosus/dienstplan) to help:

- provision a server from scratch
- set up the bot app
- deploy the app to the server

## Install

- Clone the [dienstplan](https://github.com/pilosus/dienstplan) repo, compile the `jar` as described in the documenation
- Clone the [dienstplan-deploy](https://github.com/pilosus/dienstplan-deploy/) repo at the same directory level as with `dienstplan`:

```
├── dienstplan
└── dienstplan-deploy
```
- Checkout to a separate branch if you plan to edit configuration files

```
cd dienstplan-deploy
git checkout -b your-branch
```

- Get `Python 3`
- Install Python requirements:

```
cd dienstplan-deploy
pip install -r requirements.txt
```

- Get a server (for a hobby project a DigitalOcean's Ubuntu with 1 GB Memory droplet will be just fine)
- Set up your `ssh` keys to the server
- Set up a domain/subdomain for the app to be resolved to your server's address
- Get a PosgreSQL 9.4+ server (or provision one with a playbook)
- Set up [Sentry account](https://sentry.io/)
- Set up ansible's `hosts` file, e.g. in `~/.ansible/hosts`:

```
[digital_ocean]
dienstplan ansible_host=YOUR-SEVER-IP-ADDRESS
```

## Usage

### Encrypt / descrypt vault values

See the [docs](https://docs.ansible.com/ansible/latest/user_guide/vault.html)

```
# encrypt
ansible-vault encrypt_string --vault-password-file ~/.ansible/.dienstplan_pass.txt 'String to encrypt' --name 'variable_name'

# decrypt specific variable
ansible localhost -m ansible.builtin.debug -a var="env_slack_token" -e "@vars/env.yml" --vault-password-file ~/.ansible/.dienstplan_pass.txt
```

### Provision a server

1. Update `vars/server.yml` variables, e.g. `server_domain_name`, `server_admin_email` (see above how to encrypt/decrypt a variable), `copy_local_key`
2. Provision a server with Java, Nginx server, firewall, etc:

```
ansible-playbook --limit dienstplan playbook-server-init.yml --inventory ~/.ansible/hosts --user root --vault-password-file ~/.ansible/.dienstplan_pass.txt
```

### (optional) Initialize a PostgreSQL database

1. Make sure DB credentials are set in `vars/env.yml`
2. Run the playbook:

```
# Run all tasks
ansible-playbook --limit dienstplan playbook-psql-init.yml --inventory ~/.ansible/hosts --user root --vault-password-file ~/.ansible/.dienstplan_pass.txt

# Run tasks with selected tags
ansible-playbook -l dienstplan playbook-psql-init.yml --inventory ~/.ansible/hosts -u root --vault-password-file ~/.ansible/.dienstplan_pass.txt --tags "db"
```

### Initialize the app

1. Update `vars/env.yml` (encrypt your app's environment variables, e.g. DB credentials, Slack tokens, etc.)
2. Update `vars/app.yml` (**Set app_version value to the correct version of the app that you've compiled!**)
3. Run the playbook:

```
# Run all tasks
ansible-playbook --limit dienstplan playbook-app-init.yml --inventory ~/.ansible/hosts --user root --vault-password-file ~/.ansible/.dienstplan_pass.txt

# Run tasks with selected tags
ansible-playbook -l dienstplan playbook-app-init.yml --inventory ~/.ansible/hosts -u root --vault-password-file ~/.ansible/.dienstplan_pass.txt --tags "env"
```


### Deploy the app

1. Update `var/env.yml` if needed
2. Update `var/app.yml` (do not forget to set the correct version in `app_version`)
3. Run the playbook:

```
ansible-playbook -l dienstplan playbook-app-deploy.yml --inventory ~/.ansible/hosts -u root --vault-password-file ~/.ansible/.dienstplan_pass.txt
```

Or simply pass in `app_version` as the command line option:

```
ansible-playbook --limit dienstplan playbook-app-deploy.yml \
  --inventory ~/.ansible/hosts \
  -u root \
  --vault-password-file ~/.ansible/.dienstplan_pass.txt \
  -e app_version=0.2.11
```

## Gather facts about server

Get info about `dienstplan` server from the `hosts` file:

```
ansible dienstplan --module-name setup --inventory ~/.ansible/hosts --user root
```

### Ping servers

Ping all servers of group `digital_ocean` from the `hosts` file:

```
ansible digital_ocean --inventory ~/.ansible/hosts --module-name ping --user root
```

## Debugging

```
# Log in
ssh you@your-server

# Become root
sudo -i

# Check app's files
ls -lth /opt/dienstplan/

# Check service unit files
less /etc/systemd/system/dienstplan-server.service

# See service status
systemct status dienstplan-server.service
systemct is-active dienstplan-server.service

# Reload systemd daemon
systemctl daemon-reload

# Restart service
systemct restart dienstplan-server.service

# See service logs
journalctl -u dienstplan-server.service --no-pager --since today --follow
```
