# CentOS 7/8 & Fedora Apache Server Installation and Configuration Role

A very simple role to install and configure a basic instance of Apache web server on CentOS and Fedora.

The only customization for now is some basic performance tuning for low resource servers.

```conf
KeepAlive Off

<IfModule prefork.c>
    StartServers        4
    MinSpareServers     20
    MaxSpareServers     40
    MaxClients          200
    MaxRequestsPerChild 4500
</IfModule>
```

## Requirements

None.

## Role Variables

Add and customize the following role variables in one of the following locations:

Recommended:

- host_vars/{{ HOSTNAME }}.yml
- group_vars/{{ GROUPNAME }}.yml

Optional:

- {{ roles_path }}/mariuszczyz.centos_apache/defaults/main.yml

Replace `{{ HOSTNAME }}` and `{{ GROUPNAME }}` with appropriate
inventory names.

It's recommended to add all required variables to `hosts_vars` and
`group_vars`. This way they will not get overwritten next time the
original role is updated.

| Variable | Comment | Example |
| -------- | ------- | ------- |
| START_SERVERS | initial number of threads to spawn on service restart | 4 |
| MIN_SPARE_SERVERS | number of threads to keep warm | 20 |
| MAX_SPARE_SERVERS | max number of threads to keep warm following a traffic spike | 40 |
| MAX_CLIENTS | maximum number of connections | 200 |
| MAX_REQUEST_PER_CHILD | max number of request to handle per thread before killing it | 4500 |
| |

## Dependencies

None.

## Example Playbook

### Manual

Fetch this role from Ansible Galaxy manually:

`ansible-galaxy install mariuszczyz.centos_apache`

### Not Manual

#### Galaxy

Or include this role from Ansible Galaxy via `requirements.yml`

```yaml
# requirements.yml
# Install from Ansible Galaxy
- src: mariuszczyz.centos_apache
```

#### Github option

```yaml
# requirements.yml
# Install from Github repository
- src: https://www.github.com/mariuszczyz/centos_apache
```

Then run this to install all dependencies from Ansible Galaxy:

`ansible-galaxy install -r requirements.yml`

### Run it

If you want to run this role individually create a new file:
`playbook.yml` (name it however you wish btw) with the following content:

```yaml
- hosts: servers
  user: YOUR USER
  become: True

  roles:
    - { role: mariuszczyz.centos_apache, tags: ['centos_apache'] }
```

Run it:

`ansible-playbook -i hosts playbook.yml`

## License

BSD

## Author Information

Author: Mariusz Czyz  
Date: 12/2019  
mariuszczyz.com
