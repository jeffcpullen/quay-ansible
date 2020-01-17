#  quay-ansible

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
ansible-playbook deploy.yml -i hosts -b -e QUAY_CONFIG_TAR=<path to quay config tarball>
```



Role Variables
--------------

Review the variables stored under group_vars/all to ensure that they align with your desired configurations. Default passwords are included here and should be changed or overwritten at run time.

```
* `QUAY_IMAGE_VERSION`: `"quay.io/redhat/quay:v3.1.3"` - The source and tag of Quay container
* `docker_login_quay`: `"undefined"` - This optional value is pulled from the Red Hat solution to access quay images and the value is the entire login command such as: docker login -u='REDHAT_QUAY_USER' -p='LONG_UUID_STRING' quay.io" docker_login_quay:
* `docker_login_redhat`: `"docker login -u='{{ UPSTREAM_REGISTRY_USERNAME}}' -p='{{ UPSTREAM_REGISTRY_PASSWORD }}' https://registry.redhat.io"` - Command to access the Red Hat registry, built with UPSTREAM_REGISTRY_USERNAME and UPSTREAM_REGISTRY_PASSWORD variables
* `QUAY_DATABASE_CONTAINERIZE`: `false` - Boolean if databases should be deployed as RPMs or Containers
* `QUAY_MYSQL_DEPLOY`: `true` - Boolean on if mysql should be deployed for Quay
* `MYSQL_DATABASE`: `"enterpriseregistrydb"` - The database name to be created / used for quay
* `MYSQL_PASSWORD`: `"changeme"` - The mysql database password to be set/used
* `MYSQL_ROOT_PASSWORD`: `"changeme"` - The mysql root password
* `MYSQL_USER`: `"quayuser"` - The mysql database user
* `QUAY_POSTGRESQL_DEPLOY`: `true` - Boolean on if Postgresql should be deployed for Clair / Quay
* `POSTGRES_PORT_5432_TCP_PORT`: `5432` - Port to access postgres on
* `postgresql_quay_db`: `clair` - The postgres clair database
* `postgresql_quay_user`: `quay` - The postgres clair database user
* `postgresql_quay_user_password`: `changeme` - The postgres quay user password
* `postgresql_user`: `postgres` - The postgres admin user
* `QUAY_ENDPOINT`: `"quay-server.example.com"` - The FQDN for your quay system
* `QUAY_CONFIG_PASSWORD`: `'changeme'` - The password for the quay config UI
* `QUAY_CONFIG_PORT`: `'8443'` - The port to access the quay configuration pod
* `QUAY_CONFIG_TAR`: `'undefined'` - After running the quay configuration download the tarball and set the path on this variable QUAY_CONFIG_TAR:  "PATH_TO_CONFIG_TAR_FILE"
* `QUAY_HTTP_PORT`: `'80'` - The port to access Quay if TLS is not enabled
* `QUAY_HTTPS_PORT`: `'443'` - The port to access Quay when TLS is enabled
* `CLAIR_ENDPOINT`: `'{{ QUAY_ENDPOINT }}'` - The FQDN for your clair server, defaults to same as quay
* `QUAY_CLAIR_HTTP_NO_PROXY`: `'undefined'` - list of systems not to proxy (standard NO_PROXY syntax) QUAY_CLAIR_HTTP_NO_PROXY: "{{ QUAY_ENDPOINT }}"
* `QUAY_CLAIR_HTTP_PROXY`: `'undefined'` - The address and port to http proxy server QUAY_CLAIR_HTTP_PROXY: http://proxy.example.com:8080
* `QUAY_CLAIR_HTTPS_PROXY`: `'undefined'` - The address and port to https proxy server QUAY_CLAIR_HTTPS_PROXY: http://proxy.example.com:8080
* `QUAY_REDIS_DEPLOY`: `'true'` - Boolean to decide if Redis should be deployed
* `quay_redis_password`: `'lkajsdlfkjwoeiruewoijdlkf'` - The password to set on deployed Redis
* `QUAY_REDIS_CONTAINERIZE`: `'true'` - If deployed Redis should be containerized
* `QUAY_ROOT_DIR`: `'/opt/quay'` - The directory where quay and clair configurations will be placed
* `QUAY_STORAGE_DIR`: `'unset'` - Optional extra directory to create that can hold container images QUAY_STORAGE_DIR: "{{ QUAY_ROOT_DIR }}/storage"
* `UPSTREAM_REGISTRY_PASSWORD`: `'jdoe'` - Username for the add repo playbook - should be replaced soon
* `UPSTREAM_REGISTRY_USERNAME`: `'password'` - Passowrd for the add repo playbook - should be replaced soon
* `QUAY_AUTH_TOKEN`: `'undefined'` - This is used for the api calls to populate the registry post install QUAY_AUTH_TOKEN: 'undefined'
* `SATELLITE_REGISTER`: `false` - This enables the satellite registration portion of the role that will be removed soon
* `quay_ansible_satellite_fqdn`: `'satellite.example.com'` - DEPRECATED Satellite FQDN
* `quay_ansible_satellite_key`: `'example-key'` - DEPRECATED Satellite key
* `quay_ansible_satellite_org`: `'example-org'` - DEPRECATED Satellite org
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
