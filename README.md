- name: Making directory for prometheus (Ubuntu)
  file:
    path: ~/prometheus
    state: directory

- name: Download and Extract Prometheus (Ubuntu)
  unarchive:
    src: https://github.com/prometheus/prometheus/releases/download/v2.43.0/prometheus-2.43.0.linux-amd64.tar.gz
    dest: ~/prometheus
    remote_src: yes
    mode: 0755
    owner: root
    group: root
  register: command_output
- debug:
	var: command_output.stdout_lines

- name: Stopping current service
  tags: serviceon
  service:
    name: prometheus
    state: stopped

- name: Executables(Ubuntu)
  shell: |
    cd ~/prometheus/prometheus*
    cp -r . /usr/local/bin/prometheus

- name: Setup Service File(Ubuntu)
  copy:
    src: prometheus.service
    dest: /etc/systemd/system/
    mode: 0755
    owner: root
    group: root

- name: starting and enabling prometheus (Ubuntu)
  systemd:
    name: prometheus
    state: started
    daemon_reload: true
    enabled: true
