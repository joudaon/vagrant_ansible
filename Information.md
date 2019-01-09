### Running a playbook

```sh
$> ansible-playbook <playbookname.yml>
```

### Gathering facts

- [Ansible documentation](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html)

```sh
$> ansible -m setup loadbalancer
$> ansible -m setup <hostname> -a "filter=<factname>"
$> ansible -m setup loadbalancer -a "filter=ansible_os_family"
$> ansible -m setup loadbalancer -a "filter=ansible_distribution_release"
```

#### Creating a skeleton role:
- [Ansible documentation](https://docs.ansible.com/ansible/latest/reference_appendices/galaxy.html#ansible-galaxy)
```sh
$> cd roles
$> ansible-galaxy init <rolename>
```

### Debugging
In the tasks file we add "debug task":

```sh
- debug: var=hostvars['loadbalancer']["ansible_enp0s8"]["ipv4"]["address"]
- debug: var=hostvars['loadbalancer']["ansible_facts"]["os_family"]
- debug: var=hostvars['loadbalancer']["ansible_facts"]["nodename"]
```

Then we run the playbook (with start at task we indicate in which task we want to start):
```sh
$> ansible-playbook loadbalancer.yml --start-at-task "copy custom web"
```

We should see something like this:
```sh
TASK [nginx : debug] ***********************************************************************************************************
ok: [loadbalancer] => {
    "hostvars['loadbalancer'][\"ansible_enp0s8\"][\"ipv4\"][\"address\"]": "10.0.1.11"
}

TASK [nginx : debug] ***********************************************************************************************************
ok: [loadbalancer] => {
    "hostvars['loadbalancer'][\"ansible_facts\"][\"os_family\"]": "Debian"
}

TASK [nginx : debug] ***********************************************************************************************************
ok: [loadbalancer] => {
    "hostvars['loadbalancer'][\"ansible_facts\"][\"nodename\"]": "loadbalancer"
}
```