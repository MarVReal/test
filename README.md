---
- name: Install Nagios
  hosts: all
  become: yes

  roles:
    - common
    - nagios

- name: Configure Nagios Core for email notifications
  tasks:
    - name: Create contacts.cfg file
      file:
        path: /usr/local/nagios/etc/objects/contacts.cfg
        state: touch
        mode: '0644'

    - name: Configure Nagios Core for email notifications (RedHat)
      lineinfile:
        path: /usr/local/nagios/etc/objects/contacts.cfg
        regexp: '^(.*email.*)(;)(.*)$'
        line: '\1\3'
      when: ansible_os_family == 'RedHat'

    - name: Configure Nagios Core for email notifications (Debian)
      lineinfile:
        path: /usr/local/nagios/etc/objects/contacts.cfg
        regexp: '^(.*pager.*)(;)(.*)$'
        line: '\1\3'
      when: ansible_os_family == 'Debian'

  when: nagios_installed.changed
  become: yes

- name: Set Nagios Core password
  tasks:
    - name: Create htpasswd.users file
      file:
        path: /usr/local/nagios/etc/htpasswd.users
        state: touch
        mode: '0644'

    - name: Set Nagios Core password
      shell: htpasswd -b /usr/local/nagios/etc/htpasswd.users nagiosadmin nagiosadmin
      args:
        executable: /bin/bash

  when: nagios_installed.changed
  become: yes
