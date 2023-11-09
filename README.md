---
- name: Set up a firewall on Ubuntu and CentOS 9
 hosts: all
 become: yes
 tasks:
    - name: Install and enable firewalld
      ansible.builtin.package:
        name: firewalld
        state: present
      when: ansible_os_family == 'RedHat'

    - name: Enable and start firewalld service
      ansible.builtin.systemd:
        name: firewalld
        enabled: yes
        state: started
      when: ansible_os_family == 'RedHat'

    - name: Open firewall ports for SSH, HTTP, and HTTPS
      ansible.builtin.firewalld:
        port: "{{ item }}/tcp"
        permanent: yes
        state: enabled
      with_items:
        - 22
        - 80
        - 443
      when: ansible_os_family == 'RedHat'

    - name: Install and enable UFW
      ansible.builtin.package:
        name: ufw
        state: present
      when: ansible_os_family == 'Debian'

    - name: Enable and start UFW service
      ansible.builtin.systemd:
        name: ufw
        enabled: yes
        state: started
      when: ansible_os_family == 'Debian'

    - name: Open firewall ports for SSH, HTTP, and HTTPS
      ansible.builtin.ufw:
        port: "{{ item }}"
        proto: tcp
        state: enabled
      with_items:
        - 22
        - 80
        - 443
      when: ansible_os_family == 'Debian'
