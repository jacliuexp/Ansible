## Ansible
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
