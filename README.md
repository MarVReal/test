fatal: [192.168.56.103]: FAILED! => {"changed": false, "msg": "ERROR: Exception caught: 'function' object has no attribute 'get_zone'"}

- name: Opening port for elasticsearch
  firewalld:
    port: 5601/tcp
    zone: public
    permanent: yes
    state: enabled

fatal: [192.168.56.103]: FAILED! => {"changed": false, "commands": ["/usr/sbin/ufw status verbose", "/usr/bin/grep -h '^### tuple' /lib/ufw/user.rules /lib/ufw/user6.rules /etc/ufw/user.rules /etc/ufw/user6.rules /var/lib/ufw/user.rules /var/lib/ufw/user6.rules", "/usr/sbin/ufw --version", "/usr/sbin/ufw allow from any to any port 5601/tcp"], "msg": "ERROR: Bad port '5601/tcp'\n"}

