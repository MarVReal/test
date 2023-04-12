- name: Making directory for prometheus (CentOS)
  file:
    path: ~/prometheus
    state: directory

- name: Download and Extract Prometheus (CentOS)
  tags: source
  unarchive:
    src: https://github.com/prometheus/prometheus/releases/download/v2.43.0/prometheus-2.43.0.linux-amd64.tar.gz
    dest: ~/prometheus
    remote_src: yes
    mode: 0777
    owner: root
    group: root

- name: Stopping current service
  tags: serviceon
  service:
    name: prometheus
    state: stopped

- name: Adding Executables (CentOS)
  tags: executables
  shell: |
    cd ~/prometheus/prometheus*
    cp -r . /usr/local/bin/prometheus

- name: Copying Service File
  tags: servicefile
  copy:
    src: prometheus.service
    dest: /etc/systemd/system/
    owner: root
    group: root
    mode: 777

- name: Verifying installation
  tags: serviceon
  service:
    name: prometheus
    state: restarted
    enabled: true
