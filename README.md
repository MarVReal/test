---
- name: Install Nagios Core
  get_url:
    url: https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.6.tar.gz
    dest: /tmp/nagios-4.4.6.tar.gz

- name: Extract Nagios Core archive
  unarchive:
    src: /tmp/nagios-4.4.6.tar.gz
    dest: /tmp
    remote_src: yes

- name: Change ownership of Nagios directory
  become: yes
  become_user: root
  file:
    path: /usr/local/nagios
    owner: nagios
    group: nagios
    recurse: yes

- name: Configure Nagios Core
  command: ./configure --with-httpd-conf=/etc/httpd/conf.d
  args:
    chdir: /tmp/nagios-4.4.6

- name: Build and install Nagios Core
  command: make all
  args:
    chdir: /tmp/nagios-4.4.6

- name: Install Nagios Core
  command: make install
  args:
    chdir: /tmp/nagios-4.4.6

- name: Install Nagios Plugins
  unarchive:
    src: https://nagios-plugins.org/download/nagios-plugins-2.3.3.tar.gz
    dest: /tmp
    remote_src: yes

- name: Extract Nagios Plugins archive
  unarchive:
    src: /tmp/nagios-plugins-2.3.3.tar.gz
    dest: /tmp
    remote_src: yes

- name: Configure Nagios Plugins
  command: ./configure --with-nagios-user=nagios --with-nagios-group=nagios
  args:
    chdir: /tmp/nagios-plugins-2.3.3

- name: Build and install Nagios Plugins
  command: make
  args:
    chdir: /tmp/nagios-plugins-2.3.3

- name: Install Nagios Plugins
  command: make install
  args:
    chdir: /tmp/nagios-plugins-2.3.3
