Description
===========

This is a template for deploying a PHP application from a git repo under
mod_php on multiple Linux servers with [OpenStack
Heat](https://wiki.openstack.org/wiki/Heat) on the [Rackspace
Cloud](http://www.rackspace.com/cloud/), with a [Cloud
Database](http://www.rackspace.com/cloud/databases/) back end. This template is
leveraging [chef-solo](http://docs.opscode.com/chef_solo.html) to set up the
server.

Requirements
============
* A Heat provider that supports the Rackspace `OS::Heat::ChefSolo` plugin.
* An OpenStack username, password, and tenant id.
* [python-heatclient](https://github.com/openstack/python-heatclient)
`>= v0.2.8`:

```bash
pip install python-heatclient
```

We recommend installing the client within a [Python virtual
environment](http://www.virtualenv.org/).

Example Usage
=============
Here is an example of how to deploy this template using the
[python-heatclient](https://github.com/openstack/python-heatclient):

```
heat --os-username <OS-USERNAME> --os-password <OS-PASSWORD> --os-tenant-id \
  <TENANT-ID> --os-auth-url https://identity.api.rackspacecloud.com/v2.0/ \
  stack-create myphpapp -f php_app_multi.yaml \
  -P repo=https://github.com/MyUser/MyApp.git -P url=http://myapp.org \
  -P db_user=username
```

* For UK customers, use `https://lon.identity.api.rackspacecloud.com/v2.0/` as
the `--os-auth-url`.

Optionally, set environment variables to avoid needing to provide these
values every time a call is made:

```
export OS_USERNAME=<USERNAME>
export OS_PASSWORD=<PASSWORD>
export OS_TENANT_ID=<TENANT-ID>
export OS_AUTH_URL=<AUTH-URL>
```

Parameters
==========
Parameters can be replaced with your own values when standing up a stack. Use
the `-P` flag to specify a custom parameter.

* `load_balancer_hostname`: Hostname to give the Cloud Load Balancer (Default:
  PHP-Load-Balancer)
* `server_hostname`: Hostname to give the Cloud Servers (Default: php)
* `server_count`: Number of Cloud Servers to deploy (Default: 2)
* `image`: Operating system to use (Default: Ubuntu 12.04 LTS (Precise
  Pangolin))
* `flavor`: Server size to use (Default: 4 GB Performance)
* `db_flavor`: Amount of memory for the Cloud Database to use (Default: 1GB
  Instance)
* `db_user`: User name to associate with the Cloud Database, the password is
  automatically generated and will be provided in the outputs section.
  (Default: db_user)
* `db_size`: Disk size, in GB, for the Cloud Database (Default: 10)
* `ssh_keypair_name`: Name of the SSH key pair to register with nova (Default:
  none)
* `revision`: Git tag or commit hash that should be deployed (Deafult:
  HEAD)
* `packages`: Comma delimited list of additional system packages to install
  (Default: none)
* `repo`: Specifies a git URL from which to reploy the app (Default: none)
* `url`: Vase url for your application (Default: http://example.com)
* `deploy_key`: Private key to deploy from a private git repo.
* `destination`: Directory into which the application will be installed.
  (Default: /var/www/vhosts/application)
* `public`: URL path on which the application is accessed (Default: /)
* `http_port`: Port where http connections are accepted (Default: 80)
* `https_port`: Port where https connections are accepted (Default: 443)
* `memcached_size`: Amount of memory, in MB, for memcached to use (Default:
  128)

Outputs
=======
Once a stack comes online, use `heat output-list` to see all available outputs.
Use `heat output-show <OUTPUT NAME>` to get the value fo a specific output.

* `private_key`: SSH private that can be used to login as root to the servers.
* `load_balancer_ip`: IP of the Load Balancer created with this deployment.
* `database_id`: UUID of the Cloud Database created.
* `database_password`: Password for the Cloud Databases instance.
* `server_public_ips`: Public IP addresses of the Cloud Servers created.
* `server_private_ips`: Private IP addresses of the Cloud Servers created.

For multi-line values, the response will come in an escaped form. To get rid of
the escapes, use `echo -e '<STRING>' > file.txt`. For vim users, a substitution
can be done within a file using `%s/\\n/\r/g`.

Stack Details
=============
By default the application will be deployed under /var/www/application

Contributing
============
There are substantial changes still happening within the [OpenStack
Heat](https://wiki.openstack.org/wiki/Heat) project. Template contribution
guidelines will be drafted in the near future.

License
=======
```
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
