## Ansible
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


