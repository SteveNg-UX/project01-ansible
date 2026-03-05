+--------------------------+
| server naming convention |
+--------------------------+---------------------------------------------------------+
+-----------+   +-----------+   +-----------+   +---------------+   +---------------+|
| [type]    |   | [number]  |   | [role]    |   | [environment] |   | [location]    ||
+-----------+   +-----------+   +-----------+   +---------------+   +---------------+|
| srv       |   | [0-100]   |   | db        |   | dev           |   | paris         ||
+-----------+   +-----------+   +-----------+   +---------------+   +---------------+|
| vm        |                   | web       |   | staging       |   | boston        ||
+-----------+                   +-----------+   +---------------+   +---------------+|
| ctn       |                   | proxy     |   | prod          |   | london        ||
+-----------+                   +-----------+   +---------------+   +---------------+|
|                               | bastion   |                       | pekin         ||
|                               +-----------+                       +---------------+|
|                               | app       |                       | seoul         ||
|                               +-----------+                       +---------------+|
|                                                                   | tokyo         ||
|                                                                   +---------------+|
+------------------------------------------------------------------------------------+


+----------+
| srv pool |
+----------+--------+---------------+---------------+---------------+-----------+
| host              | ipv4          | server groups | environment   | site      |
+-------------------+---------------+---------------+---------------+-----------+
| srv01_orchestrator| 10.0.1.10 /24 | srv_db_prod   | production    | paris     |
| srv01_db_prod     | 10.0.1.11 /24 | srv_db_prod   | production    | paris     |
| srv01_web_prod    | 10.0.1.12 /24 | srv_web_prod  | production    | paris     |
| srv02_db_prod     | 10.0.1.13 /24 | srv_db_prod   | production    | paris     |
| srv02_web_prod    | 10.0.1.14 /24 | srv_web_prod  | production    | paris     |
| srv01_db_dev      | 10.0.1.15 /24 | srv_db_dev    | development   | paris     |
| srv01_web_dev     | 10.0.1.16 /24 | srv_web_dev   | development   | paris     |
| srv02_db_dev      | 10.0.1.17 /24 | srv_db_dev    | development   | paris     |
| srv02_web_dev     | 10.0.1.18 /24 | srv_web_dev   | development   | paris     |
+-------------------+---------------+---------------+---------------+-----------+


+-----------------------------------------------+-----------------------------------------------------------+
| arborescence template                         | fonction                                                  |
+-----------------------------------------------+-----------------------------------------------------------+
| inventories/[environment]/group_vars/all.yml  | default ansible vars for hosts (user by default)          |
| inventories/[environment]/group_vars/srv_.yml | (optional) business logic variables (pour les template)   |
| inventories/[environment]/host_vars/srv.yml   | specific ansible vars for each hosts (user, ssh_key, port)|
| inventories/[environment]/hosts.yml           | hierarchie hosts (groups hosts, hosts)                    |
| roles/[role]/tasks/main.yml                   | role-based tasks                                          |
| playbook.yml                                  | root tasks witch launch role tasks                        |
+-----------------------------------------------+-----------------------------------------------------------+


+-------+-----------------------------------+-----------------------------------------------+
| step  | description                       | command                                       |
+-------+-----------------------------------+-----------------------------------------------+
| 1     | install ansible                   | apt install ansible -y                        |
| 1     | check inventory                   | ansible-inventory -i inventories --graph      |
| 2     | check synthax playbook            | ansible-playbook playbook.yml -i inventories  |
| 3     | check hosts communications        | ansible all -m ping -u ansible                |
+-------+-----------------------------------+-----------------------------------------------+