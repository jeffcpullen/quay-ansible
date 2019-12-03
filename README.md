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

 + To run this with the included Satellite registration parts run the following command
 ```
 ansible-playbook -i hosts deploy.yml -b
 ```
 
 + To run this without the satellite registration parts add the following flag `--skip-tags register` and ensure the systems have the following repositories available

 ```
       - rhel-7-server-rpms
       - rhel-7-server-extras-rpms
       - rhel-server-rhscl-7-rpms (if not deploying containerized)
```

+ If this is the first time that you've installed Quay, you will need to go through the Quay configuration mode to create your config tarball. Do this by adding the following flag to the playbook `-e QUAY_CONFIG=true`

+ If you've already run the config mode, or have a config tarball you can run the full deployment by referencing a configuration tarball. After running through the Quay config WebUI, copy the downloaded tarball to the ansible host and reference it with `-e QUAY_CONFIG_TAR=<path to tarball>`

Note to self, make this more clear

```
ansible-playbook deploy.yml -i hosts -b -e QUAY_CONFIG=true
```


```
ansible-playbook deploy.yml -i hosts -b -e QUAY_CONFIG_TAR=~/quay-config.tar.gz
```



Role Variables
--------------

Review the variables stored under group_vars/all to ensure that they align with your desired configurations. Default passwords are included here and should be changed or overwritten at run time.

```
MYSQL_CONTAINER_NAME: mysql
MYSQL_DATABASE: enterpriseregistrydb
MYSQL_PASSWORD: JzxCTamgFBmHRhcGFtoPHFkrx1BH2vwQ
MYSQL_USER: quayuser
MYSQL_ROOT_PASSWORD: L36PrivxRB02bqOB9jtZtWiCcMsApOGn
QUAY_CONFIG_PASSWORD: my-secret-password
QUAY_CONFIG_PORT: 8443
QUAY_OBJECT_STORAGE: false
QUAY_HTTP_PORT: 80
QUAY_HTTPS_PORT: 443
QUAY_REDIS_CONTAINERIZE: true
QUAY_DATABASE_CONTAINERIZE: true

# Choose one or the other
QUAY_MYSQL_DEPLOY: true
QUAY_POSTGRESQL_DEPLOY: false
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
