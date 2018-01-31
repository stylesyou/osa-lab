## On Deployment Node

1. Clone rpc repo to deployment node
```sh
  git clone https://github.com/rcbops/rpc-openstack /opt/rpc-openstack
```
2. cd /opt/rpc-openstack
```sh
  export RPC_PRODUCT_RELEASE="newton"
  ./scripts/deploy.sh
```
3. Update package source lists:

  ```sh 
  $ apt-get update
  ## Upgrade the system packages and kernel:
 
  $ apt-get dist-upgrade
  ## Reboot the host.
  ```
4. Install additional software packages if they were not installed during the operating system installation:

 ```sh
  $ apt-get install aptitude build-essential git ntp ntpdate \
    openssh-server python-dev sudo
  ```
5. run the Ansible bootstrap script:

  ```sh
    cd /opt/rpc-openstack
    export RPC_PRODUCT_RELEASE="Newton"  # This is optional, if unset the current stable product will be used
    openstack-ansible openstack-ansible-install.yml

  (or)

  Change to the /opt/openstack-ansible directory, and
    # scripts/bootstrap-ansible.sh
  ```
5. cp all confi files to /etc/openstack_deploy

  ```sh
  cp -r /opt/openstack-ansible/etc/openstack_deploy /etc/

  ```
6. Customize openstack_user_config.yml and user_variables.yml file as per the needs

  -> Set HAProxy to use http if using internal and external as same IP
  ```sh
      vim /etc/openstack_deploy/user_variables.yml

      haproxy_ssl: false
  ```
      
7. Generate random passwords

  ```sh
  cd /opt/openstack-ansible/scripts
  python pw-token-gen.py --file /etc/openstack_deploy/user_secrets.yml

 ```
8. Artifact Setup

```sh
  cd /opt/rpc-openstack
  export RPC_PRODUCT_RELEASE="newton"
  openstack-ansible site-artifacts.yml
```
9. Change to the /opt/openstack-ansible/playbooks directory.

10. Run the host setup playbook:

  ```sh 
  $ openstack-ansible setup-hosts.yml 
  ```

11. Run the infrastructure setup playbook:

  ```sh 
  $ openstack-ansible setup-infrastructure.yml 
  ```

12. Run the following command to verify the database cluster:

  ```sh
  $ ansible galera_container -m shell \
    -a "mysql -h localhost -e 'show status like \"%wsrep_cluster_%\";'"
  ```
13. Run the OpenStack setup playbook:

   ```sh  
   $ openstack-ansible setup-openstack.yml 
   ```

