fatal: [192.168.56.103]: FAILED! => {"changed": false, "msg": "ERROR: Exception caught: 'function' object has no attribute 'get_zone'"}

- name: Opening port for elasticsearch
  firewalld:
    port: 5601/tcp
    zone: public
    permanent: yes
    state: enabled
