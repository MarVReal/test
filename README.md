fatal: [192.168.56.103]: FAILED! => {"changed": true, "cmd": "firewall-cmd --permanent --zone=public --add-port=9600/tcp\nsleep 10\nfirewall-cmd --reload\n", "delta": "0:00:10.005251", "end": "2023-04-21 08:42:17.645914", "msg": "non-zero return code", "rc": 127, "start": "2023-04-21 08:42:07.640663", "stderr": "/bin/sh: 1: firewall-cmd: not found\n/bin/sh: 3: firewall-cmd: not found", "stderr_lines": ["/bin/sh: 1: firewall-cmd: not found", "/bin/sh: 3: firewall-cmd: not found"], "stdout": "", "stdout_lines": []}


- name: Opening port for Logstash (CentOS)
  become: true
  shell: |
    firewall-cmd --permanent --zone=public --add-port=9600/tcp
    sleep 10
    firewall-cmd --reload
  when: ansible_distribution == "CentOS"
