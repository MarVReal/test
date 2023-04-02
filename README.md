---
- name: Restart Apache
  service:
    name: httpd
    state: restarted
  when: ansible_os_family == 'RedHat'

- name: Restart Apache
  service:
    name: apache2
    state: restarted
  when: ansible_os_family == 'Debian'

- name: Restart Nagios Core
  service:
    name: nagios
    state: restarted
