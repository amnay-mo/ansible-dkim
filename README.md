# ansible-dkim
Ansible role for opendkim with postfix configuration on Debian

### Example playbook
```yaml
---
- name: Installing and configuring postfix and opendkim
  hosts: mail
  remote_user: root
  vars_files: ['var_mail.yml']
  pre_tasks:
    - name: Ensuring /etc/opendkim/keys directory
      file: path=/etc/opendkim/keys state=directory 

    - name: Pushing DKIM private keys
      copy: src=keys/{{ item.key }} dest=/etc/opendkim/keys mode=0400 # mode 0400 is mandatory otherwise opendkim won't use the key
      with_items: "{{ dkim_domains }}"
  roles:
  - { role: ansible-dkim }

```
### `var_mail.yml`:


```yaml
---
---
admin_email: 'amnay.mokhtari@web-education.net'
dkim_domains: 
  - domain: 'neoscol.com'
    selector: 'mail'
    key: 'mail.private'
  - domain: 'amnay.fr'
    selector: 'amnay'
    key: 'amnay.private'
opendkim_iface: '172.17.0.2'
opendkim_trusted_hosts: ['localhost', '127.0.0.1', '0.0.0.0/0', '172.17.0.0/24']
postfix_mynetworks: '127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 172.17.0.0/24'
```
