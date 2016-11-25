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
         knock_firewall_type: ufw # ufw | iptables        
       }

```


Client usage
------------

```bash
knock server_ip_address 15000, 16000, 17000
ssh user@server_ip_address
```  


Copyright and license
---------------------

Copyright - Vyacheslav Voronenko

Code licensed under the [BSD 3 clause] (https://opensource.org/licenses/BSD-3-Clause) or the [MIT License] (http://opensource.org/licenses/MIT).

Subscribe for roles updates at [FB] (https://www.facebook.com/SoftAsap/)
