## Ansible
```
# show version
ansible --version
# ping remote to see if reachable with ansible
ansible all -i 172.31.0.11, -m ping    # if no comma, it will take 172.31.0.11 as inventory file instead
ansible all -i web01,web02 -m ping

ansible all -i us-d-nagios-0,nagios-0 -m command --args 'uptime'  # run uptime on 2 servers
ansible all -i us-d-nagios-0,nagios-0 -m command --args 'sudo apt-get install -y apache2'   # install apache2 on 2 servers






```

https://spacelift.io/blog/ansible-variables
```
#### Variable in Task
- name: Example Simple Variable
  hosts: all
  become: yes
  vars:
    username: bob

  tasks:
  - name: Add the user {{ username }}
    ansible.builtin.user:
      name: "{{ username }}"
      state: present
```
```
#### Magic Variable: auto created by Ansible and cannot be changed by user. Reflect the state of Ansible.
---
- name: Echo playbook
  hosts: localhost
  gather_facts: no
  tasks:
    - name: Echo inventory_hostname
      ansible.builtin.debug:
        msg:
          - "Hello from Ansible playbook!"
          - "This is running on {{ inventory_hostname }}"
```

```
#### Ansible facts
##### Getting system and hardware facts gathered about the current host during playbook execution (OS type, IP, CUP/Mem)
```

```
#### Connection Variable
---
- name: Echo message on localhost
  hosts: localhost
  connection: local
  gather_facts: no
  vars:
    message: "Hello from Ansible playbook on localhost!"
  tasks:
    - name: Echo message and connection type
      ansible.builtin.shell: "echo '{{ message }}' ; echo 'Connection type: {{ ansible_connection }}'"
      register: echo_output

    - name: Display output
      ansible.builtin.debug:
        msg: "{{ echo_output.stdout_lines }}"
```
```
#### Register Variable
- name: Example Register Variable Playbook
  hosts: all
  
  tasks:
  - name: Run a script and register the output as a variable
    shell: "find hosts"
    args:
      chdir: "/etc"
    register: find_hosts_output
  - name: Use the output variable of the previous task
    debug:
      var: find_hosts_output
```
```
#### Host vars and Group Vars
[webservers]
webserver1 ansible_host=10.0.0.1 ansible_user=user1
webserver2 ansible_host=10.0.0.2 ansible_user=user2

[webservers:vars]
http_port=80
```
```
#### the same as above
group_vars/databases 
group_vars/webservers
host_vars/host1
host_vars/host2
```
```
#### Display all the vars
---
- name: Display all variables/facts known for a host
  hosts:
  - us-d-nagios-0
  tasks:
  - debug:
      var: hostvars[inventory_hostname]
```
```
#### Display all the vars
[experian-ais-dev->ec2-user@api-0~/jack/ansiblevar]$ cat b.yml
- hosts: localhost
  gather_facts: false
  vars:
    test_var1: A
    test_var2: "{{ test_var1 }}"
  tasks:
    - debug:
        var: vars
```
