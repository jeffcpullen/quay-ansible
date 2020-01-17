#  quay-ansible


## Tags:

* `register` - 


* `database` - 


* `redis` - 


* `quay` - 


* `quay-config` - 


* `quay-worker` - 


* `quay-clair` - 


* `v3.11.98` - 


* `v3.11*` - 


* `latest` - 


* `*` - 


* `v3.11.*` - 


* `3.2.22` - 


* `v3.2*` - 


* `docker` - 


* `postgres` - 


* `deploy-config` - 

## Variables:

### Playbook:
| Variable Name | Default | Description |
| ------------- |:-------:|:-----------:|
| `MYSQL_CONTAINER_NAME`| `'mysql'` | Name to apply to mysql container |
| `postgresql_version`| `postgresql96` | The version of postgresql to deploy |
| `POSTGRES_CONNECTION_STRING`| `"host={{ QUAY_ENDPOINT }} sslmode=disable dbname={{ postgresql_quay_db }} user={{ postgresql_quay_user }} password={{ postgresql_quay_user_password }} statement_timeout=60000"` | The connection string built with other variables |
| `QUAY_IMAGE_VERSION`| `"quay.io/redhat/quay:v3.1.3"` | The source and tag of Quay container |
| `docker_login_quay`| `"undefined"` | This optional value is pulled from the Red Hat solution to access quay images and the value is the entire login command such as: docker login -u='REDHAT_QUAY_USER' -p='LONG_UUID_STRING' quay.io" docker_login_quay: |
| `docker_login_redhat`| `"docker login -u='{{ UPSTREAM_REGISTRY_USERNAME}}' -p='{{ UPSTREAM_REGISTRY_PASSWORD }}' https://registry.redhat.io"` | Command to access the Red Hat registry, built with UPSTREAM_REGISTRY_USERNAME and UPSTREAM_REGISTRY_PASSWORD variables |
| `QUAY_DATABASE_CONTAINERIZE`| `false` | Boolean if databases should be deployed as RPMs or Containers |
| `QUAY_MYSQL_DEPLOY`| `true` | Boolean on if mysql should be deployed for Quay |
| `MYSQL_DATABASE`| `"enterpriseregistrydb"` | The database name to be created / used for quay |
| `MYSQL_PASSWORD`| `"changeme"` | The mysql database password to be set/used |
| `MYSQL_ROOT_PASSWORD`| `"changeme"` | The mysql root password |
| `MYSQL_USER`| `"quayuser"` | The mysql database user |
| `QUAY_POSTGRESQL_DEPLOY`| `true` | Boolean on if Postgresql should be deployed for Clair / Quay |
| `POSTGRES_PORT_5432_TCP_PORT`| `5432` | Port to access postgres on |
| `postgresql_quay_db`| `clair` | The postgres clair database |
| `postgresql_quay_user`| `quay` | The postgres clair database user |
| `postgresql_quay_user_password`| `changeme` | The postgres quay user password |
| `postgresql_user`| `postgres` | The postgres admin user |
| `QUAY_ENDPOINT`| `"quay-server.example.com"` | The FQDN for your quay system |
| `QUAY_CONFIG_PASSWORD`| `'changeme'` | The password for the quay config UI |
| `QUAY_CONFIG_PORT`| `'8443'` | The port to access the quay configuration pod |
| `QUAY_CONFIG_TAR`| `'undefined'` | After running the quay configuration download the tarball and set the path on this variable QUAY_CONFIG_TAR:  "PATH_TO_CONFIG_TAR_FILE" |
| `QUAY_HTTP_PORT`| `'80'` | The port to access Quay if TLS is not enabled |
| `QUAY_HTTPS_PORT`| `'443'` | The port to access Quay when TLS is enabled |
| `CLAIR_ENDPOINT`| `'{{ QUAY_ENDPOINT }}'` | The FQDN for your clair server, defaults to same as quay |
| `QUAY_CLAIR_HTTP_NO_PROXY`| `'undefined'` | list of systems not to proxy (standard NO_PROXY syntax) QUAY_CLAIR_HTTP_NO_PROXY: "{{ QUAY_ENDPOINT }}" |
| `QUAY_CLAIR_HTTP_PROXY`| `'undefined'` | The address and port to http proxy server QUAY_CLAIR_HTTP_PROXY: http://proxy.example.com:8080 |
| `QUAY_CLAIR_HTTPS_PROXY`| `'undefined'` | The address and port to https proxy server QUAY_CLAIR_HTTPS_PROXY: http://proxy.example.com:8080 |
| `QUAY_REDIS_DEPLOY`| `'true'` | Boolean to decide if Redis should be deployed |
| `quay_redis_password`| `'lkajsdlfkjwoeiruewoijdlkf'` | The password to set on deployed Redis |
| `QUAY_REDIS_CONTAINERIZE`| `'true'` | If deployed Redis should be containerized |
| `QUAY_ROOT_DIR`| `'/opt/quay'` | The directory where quay and clair configurations will be placed |
| `QUAY_STORAGE_DIR`| `'unset'` | Optional extra directory to create that can hold container images QUAY_STORAGE_DIR: "{{ QUAY_ROOT_DIR }}/storage" |
| `UPSTREAM_REGISTRY_PASSWORD`| `'jdoe'` | Username for the add repo playbook - should be replaced soon |
| `UPSTREAM_REGISTRY_USERNAME`| `'password'` | Passowrd for the add repo playbook - should be replaced soon |
| `QUAY_AUTH_TOKEN`| `'undefined'` | This is used for the api calls to populate the registry post install QUAY_AUTH_TOKEN: 'undefined' |
| `SATELLITE_REGISTER`| `false` | This enables the satellite registration portion of the role that will be removed soon |
| `quay_ansible_satellite_fqdn`| `'satellite.example.com'` | DEPRECATED Satellite FQDN |
| `quay_ansible_satellite_key`| `'example-key'` | DEPRECATED Satellite key |
| `quay_ansible_satellite_org`| `'example-org'` | DEPRECATED Satellite org |
## TODO:

#### Containerized database testing:
*  (Playbook)
#### This is actually being used for clair, need to test using clair and quay on postgres:
*  (Playbook)


Documentation generated using: [Ansible-autodoc](https://github.com/AndresBott/ansible-autodoc)

