- name: Add Elasticsearch GPG key
  apt_key:
    url: https://artifacts.elastic.co/GPG-KEY-elasticsearch
    state: present
  when: ansible_os_family == 'Debian'
      
- name: Add Elasticsearch repository
  apt_repository:
     repo: "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main"
     filename: elastic-7.x.list
  when: ansible_os_family == 'Debian'
      
- name: Add Elasticsearch GPG key
  yum_key:
    url: https://artifacts.elastic.co/GPG-KEY-elasticsearch
  when: ansible_os_family == 'RedHat'
      
 - name: Add Elasticsearch repository
   yum_repository:
        name: elasticsearch
        description: Elasticsearch repository for 7.x packages
        baseurl: https://artifacts.elastic.co/packages/7.x/yum
        gpgkey: https://artifacts.elastic.co/GPG-KEY-elasticsearch
        enabled: yes
        gpgcheck: yes
   when: ansible_os_family == 'RedHat'
      
- name: Update apt cache
  apt:
    update_cache: yes
  when: ansible_os_family == 'Debian'
      
- name: Update yum cache
  yum:
    update_cache: yes
  when: ansible_os_family == 'RedHat'
      
- name: Install Elasticsearch
  package:
    name: elasticsearch
    state: present
        
- name: Configure Elasticsearch
  lineinfile:
    path: /etc/elasticsearch/elasticsearch.yml
    regexp: '^network.host:'
    line: 'network.host: localhost'
  notify:
     - restart Elasticsearch
	
- name: restart Elasticsearch
  service:
  name: elasticsearch
    state: restarted
