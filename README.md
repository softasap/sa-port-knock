sa-port-knock
=============

[![Build Status](https://travis-ci.org/softasap/sa-port-knock.svg?branch=master)](https://travis-ci.org/softasap/sa-port-knock)


Example of use: check box-example

Configuration:
```YAML

custom_knock_default_props:
    - {regexp: "^[#]?START_KNOCKD=*", line: "START_KNOCKD=1"}
    - {regexp: "^[#]?KNOCKD_OPTS=*", line: '#KNOCKD_OPTS="-i eth1"'}

custom_knock_ports:
  - {
    name: "ssh",
    sequence: "15000, 16000, 17000",
    seq_timeout: 5,
    port: 22,
    tcpflags: syn,
    cmd_timeout: 10
    }
  - {
    name: "pptp",
    sequence: "15001, 16001, 17001",
    seq_timeout: 5,
    port: 1723,
    tcpflags: syn,
    cmd_timeout: 1000
    }

```

Simple:

```YAML


     - {
         role: "sa-port-knock",
         knock_ports: "{{custom_knock_ports}}"
       }

```


Advanced:

```YAML


     - {
         role: "sa-port-knock",
         knock_ports: "{{custom_knock_ports}}",
         knock_default_props: "{{custom_knock_default_props}}",
         knock_firewall_type: ufw # ufw | iptables,
         option_close_hardened_ports: true        
       }

```


Client usage
------------

Usually I have few trusted networks, which I allow via dedicated rule using shell:

```bash
ufw allow from 192.168.0.234/24 to any port 22 proto tcp
```
or using sa-box-bootstrap role:

```YAML
ufw_rules_allow_from_hosts:
  - {
      port: 22,
      proto: tcp,
      address: 192.168.0.264
    }
```

Don't forget to disable your hardened port ,  like

```bash
ufw delete allow 22/tcp
```

or `option_close_hardened_ports` recipe setting

Test setup:

```bash

➜  telnet  192.168.0.20 22
Trying 192.168.0.20...

➜  knock 192.168.0.20 15000, 16000, 17000

➜  telnet  192.168.0.20 22               
Trying 192.168.0.20...
Connected to 192.168.0.20.
Escape character is '^]'.
SSH-2.0-OpenSSH_6.6.1p1 Ubuntu-2ubuntu2.7

➜  ssh slavko@192.168.0.20

Welcome to Ubuntu 14.04.1 LTS (GNU/Linux 3.13.0-32-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

New release '16.04.1 LTS' available.
Run 'do-release-upgrade' to upgrade to it.



```  


Copyright and license
---------------------

Copyright - Vyacheslav Voronenko

Code licensed under the [BSD 3 clause] (https://opensource.org/licenses/BSD-3-Clause) or the [MIT License] (http://opensource.org/licenses/MIT).

Subscribe for roles updates at [FB] (https://www.facebook.com/SoftAsap/)
