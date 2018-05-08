############ 
Installation
############

Puppet Master
*************

Puppet Master needs to be installed on a standalone server that has connectivity to all the Dell EMC Networking devices to be managed under Puppet. The dellemcnetworking-dellos10 Puppet module is tested against Puppet Enterprise Edition version 5.3. The dellos10 module needs to be installed on the Puppet Master server.

.. code-block:: bash

    $ puppet module install dellemcnetworking-dellos10

See `Puppet Labs: Installing Modules <https://puppet.com/docs/puppet/5.3/modules_installing.html>`_ for more information.

Puppet Agent
************

Each network device to be managed by Puppet requires a one-time installation of the Puppet agent. The ``os10_devops_infra_install.sh`` script installs the Puppet client and it's dependencies.

The user os10devops must be created with the role of sysadmin (use the ``username`` command in CONF mode in the OS10 CLI).

  .. code-block:: bash

      OS10(config)# username os10devops password <password_str> role sysadmin

Download the `os10_devops_infra_install.sh <https://raw.githubusercontent.com/Dell-Networking/dellos10-ruby-utils/master/os10_devops_infra_install.sh>`_ script.  
After downloading the script, change the permissions using ``chmod +x os10_devops_infra_install.sh``. Execute the ``os10_devops_infra_install.sh`` script to install Puppet and the devops Ruby utilities Debian package.

Usage
=====

.. code-block:: bash

    $ os10_devops_infra_install.sh puppet active_partition/standby_partition local/remote <puppet_client_url> local/remote <os10_devops_ruby_utils_url>
    $ os10_devops_infra_install.sh puppet_ruby_utils active_partition/standby_partition local/remote <os10_devops_ruby_utils_url>

**Options**

- puppet: used to install both puppet and devops infra module
  - puppet_ruby_utils: used to install only devops Ruby utilities Debian package for Puppet; Puppet client should be already installed in the switch before installing devops Ruby utilities Debian package for the ``puppet_ruby_utils`` option

- active_partition: denotes current partition
- standby_partition: denotes standby partition; prerequisites for this option:
  - OS10 image should be upgraded in the standby partition
  - Puppet client should also be installed in the loaded or active partition

- local: denotes the relative path in the switch
- remote: denotes the relative path in the remote machine using protocols such as https, ftp, and so on

- <*puppet_client_url*>: Puppet URL should be an HTTPS/FTP path if previous option is remote (for example, https://apt.puppetlabs.com/puppet5-release-jessie.deb)
  -  <*puppet_client_url*>: download the ``puppet5-release-jessie.deb`` package in the local path of the switch if previous option is local (for example, /home/admin/)

- <*os10_devops_ruby_utils_url*>: devops Ruby utilies URL link from GitHub if previous option is remote (for example, https://raw.githubusercontent.com/Dell-Networking/dellos10-ruby-utils/master/os10-devops-ruby-utils-1.0.0.deb)
  - <*os10_devops_ruby_utils_url*>: download the ``os10-devops-ruby-utils-1.0.0.deb`` package in the local path of the switch if previous option is local (for example, /home/admin/)

**Sample usage**

    ./os10_devops_infra_install.sh puppet standby_partition remote https://apt.puppetlabs.com/puppet5-release-jessie.deb local /home/admin
  
OR
  
    ./os10_devops_infra_install.sh puppet active_partition remote https://apt.puppetlabs.com/puppet5-release-jessie.deb remote https://raw.githubusercontent.com/Dell-Networking/dellos10-ruby-utils/master/os10-devops-ruby-utils-1.0.0.deb
  
OR
  
    ./os10_devops_infra_install.sh puppet active_partition local /home/admin/ local /home/admin/
  
OR
  
    ./os10_devops_infra_install.sh puppet_ruby_utils standby_partition local /home/admin
  
OR
  
    ./os10_devops_infra_install.sh puppet_ruby_utils active_partition remote https://raw.githubusercontent.com/Dell-Networking/dellos10-ruby-utils/master/os10-devops-ruby-utils-1.0.0.deb
  
> **NOTE**: After the image upgrade and reload, execute the Puppet client. If the Puppet client throws the "cannot load such file -- xml/libxml" error, execute the /opt/puppetlabs/puppet/bin/gem install libxml-ruby command in root/sudo mode.     
