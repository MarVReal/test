---
- name: Install required packages
  apt:
    name:
      - build-essential
      - libgd-dev
    state: present
  when: ansible_os_family == 'Debian'

- name: Install required packages
  yum:
    name:
      - gcc
      - glibc
      - glibc-common
      - make
      - httpd
      - php
      - gd
      - gd-devel
      - perl
      - postfix
      - wget
      - unzip
    state: present
  when: ansible_os_family == 'RedHat'

- name: Ensure Apache is started and enabled
  service:
    name: httpd
    state: started
    enabled: yes
  when: ansible_os_family == 'RedHat'
