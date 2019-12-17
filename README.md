Quay Ansible
=========

This role is intended to install Quay and its dependencies to one or more systems using the basic installation method.

Requirements
------------

Before running:

1) You'll need to pull down the roles that this playbook depends on. You may need to define an HTTP proxy for some environments.

 ```
 ansible-galaxy install --roles-path ./roles/ -r requirements.yml
 ```

2) Get the quay shared secret from https://access.redhat.com/solutions/3533201.

     If Docker is already installed on the quay host run the docker login string provided in that solution

     If Docker isn't installed yet you can add the config.json to the system in advance of Docker (which will be installed as part of this playbook)

     ```
     mkdir -p ~/.docker
     vi ~/.docker/config.json
     <paste in solution config.json>
     ```

3) Requires Ansible 2.7+ on the host executing these playbooks

4) You should have sufficient storage allocated to the node to hold the downloaded images. This directory is defined in the group_vars/all as the QUAY_STORAGE_DIR. Default is /opt/quay/storage.


Usage
-----


 + To register a system to satellite with the necessary repositories update SATELLITE_REGISTER variable via group_vars/all or by defining it as an extra variable
 ```
 -e SATELLITE_REGISTER=true
 ```

 + If you aren't using the satellite registration plays ensure the systems have the following repositories available

 ```
       - rhel-7-server-rpms
       - rhel-7-server-extras-rpms
       - rhel-server-rhscl-7-rpms (if not deploying containerized)
```

+ If this is the first time that you've installed Quay, you will need to go through the Quay configuration mode to create your config tarball. Do this by adding the following flag to the playbook `-e QUAY_CONFIG=true`

```
ansible-playbook -i hosts deploy.yml -b --skip-tags register -e QUAY_CONFIG=true
```


+ If you've already run the config mode, or have a config tarball you can run the full deployment by referencing a configuration tarball. After running through the Quay config WebUI, copy the downloaded tarball to the ansible host and reference it with `-e QUAY_CONFIG_TAR=<path to tarball>`


```
<<<<<<< HEAD
ansible-playbook deploy.yml -i hosts -b -e QUAY_CONFIG_TAR=<path to quay config tarball>
=======
ansible-playbook deploy.yml -i hosts -b -e QUAY_CONFIG_TAR=<path to quay config tarball>
>>>>>>> 1e8de2f5d1d9c804b61ef1ea2df4a4e625542ac9
```



Role Variables
--------------

Review the variables stored under group_vars/all to ensure that they align with your desired configurations. Default passwords are included here and should be changed or overwritten at run time.

```
QUAY_IMAGE_VERSION: "quay.io/redhat/quay:v3.1.3"
#QUAY_IMAGE_VERSION: "quay.io/quay/quay:v3.2.0-3"

QUAY_ENDPOINT: quay.homelab.work
CLAIR_ENDPOINT: "{{ QUAY_ENDPOINT }}"

MYSQL_CONTAINER_NAME: mysql
MYSQL_DATABASE: enterpriseregistrydb
MYSQL_PASSWORD: JzxCTamgFBmHRhcGFtoPHFkrx1BH2vwQ
MYSQL_USER: quayuser
MYSQL_ROOT_PASSWORD: L36PrivxRB02bqOB9jtZtWiCcMsApOGn
QUAY_CONFIG_PASSWORD: my-secret-password
QUAY_ROOT_DIR: /opt/quay
QUAY_STORAGE_DIR: "{{ QUAY_ROOT_DIR }}/storage"
QUAY_CONFIG_PORT: 8443
QUAY_OBJECT_STORAGE: false
QUAY_HTTP_PORT: 80
QUAY_HTTPS_PORT: 443
QUAY_REDIS_DEPLOY: true
QUAY_REDIS_CONTAINERIZE: true
QUAY_DATABASE_CONTAINERIZE: false

QUAY_MYSQL_DEPLOY: true
QUAY_POSTGRESQL_DEPLOY: true

SATELLITE_REGISTER: false

# Mysql role
mysql_root_home: /root
mysql_root_username: root
mysql_root_password: "{{ MYSQL_ROOT_PASSWORD }}"
mysql_enabled_on_startup: true
mysql_databases:
  - name: "{{ MYSQL_DATABASE }}"
mysql_users:
  - name: "{{ MYSQL_USER }}"
    host: "%"
    password: "{{ MYSQL_PASSWORD }}"
    priv: "{{ MYSQL_DATABASE }}.*:ALL"

# Postgres role
postgresql_user: postgres
postgresql_group: postgres
postgresql_unix_socket_directories:
  - /var/run/postgresql
postgresql_service_state: started
postgresql_service_enabled: true
POSTGRES_CONNECTION_STRING: postgresql://quay@localhost:5432/clair?sslmode=disable


_mysql_packages:
  default:
    - mariadb-server
    - mariadb-devel
    - python2-mysql
  RedHat-7:
    - rh-mariadb103-scldevel.x86_64
    - rh-mariadb103-syspaths
    - MySQL-python
  CentOS-7:
    - mariadb-devel
    - mariadb-server
    - MySQL-python
  CentOS:
    - mysql-devel
    - mysql-server
    - python3-PyMySQL


_mysql_service:
  default: mysql
  Alpine: mariadb
  Amazon: mariadb
  Amazon-2018: mysqld
  Archlinux: mariadb
  Fedora: mariadb
  RedHat-7: rh-mariadb103-mariadb
  RedHat: mysqld

```

Dependencies
------------

You will need to have a valid RHEL and Quay subscription, and access tokens for your accounts. Those are not configured as a part of this playbook.


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).



#### NOTES
the create repo mirror wants the organization_name+mirror for the robot name

Need a robot creation after repo creation

Need a sync start  
