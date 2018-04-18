############
Installation
############

Puppet Master
#############
Puppet Master needs to be installed on a standalone server that has connectivity to all the Dell EMC Networking devices that need to be managed under Puppet. The dellemcnetworking-dellos10 Puppet module is tested against Puppet Enterprise Edition version 5.3. The dellos10 module needs to be installed on the Puppet Master server using the following command.

.. code-block:: bash

    $ puppet module install dellemcnetworking-dellos10

For more information on installing Puppet modules see `Puppet Labs: Installing Modules <https://docs.puppetlabs.com/puppet/latest/reference/modules_installing.html>`_


Puppet Agent
############
Each network device to be managed by Puppet requires an one time installation of Puppet agent. The os10_devops_infra_install.sh script installs the puppet client and it's dependencies.

The pre-requiste for this installation is that, the user os10devops should have been created with role as sysadmin.
  e.g., execute the below command in the conf mode of OS10 CLI
      username os10devops password <password_str> role sysadmin

Download the script os10_devops_infra_install.sh in the switch, from the url path https://github.com/Dell-Networking/dellos10-ruby-utils/os10_devops_infra_install.sh
Execute the script os10_devops_infra_install.sh to install puppet and Devops ruby utils debian package.

Usage
-----

  Usage: os10_devops_infra_install.sh puppet active_partition/standby_partition local/remote <puppet_client_url> local/remote <os10_devops_ruby_utils_url>
         os10_devops_infra_install.sh puppet_ruby_utils active_partition/standby_partition local/remote <os10_devops_ruby_utils_url>
options
~~~~~~~

  1. puppet - This option is used to install both puppet and devops infra module
     puppet_ruby_utils - This option is used to install only devops ruby utils debian package for puppet. Puppet client should be already installed in the switch, before installing devops ruby utils debian package for puppet_ruby_utils option.

  2. active_partition - This option denotes current partition
     standby_partition - This option denotes standby partition. pre-requiste for this option,
                         1. OS10 Image should be upgraded in the standby partition.
                         2. The puppet client should also be installed in the loaded or active partition.

  3. local  - The option denotes the relative path in the switch
     remote - The option denotes the relative path in the remote machine using protocols like https, ftp etc

  4. <puppet_client_url> - The puppet url should be a https/ftp path, if previous option is remote
     e.g., https://apt.puppetlabs.com/puppet5-release-jessie.deb

    <puppet_client_url> - The puppet path in the switch, if previous option is local
     e.g., /home/admin/

  4. <os10_devops_ruby_utils_url> - The devops ruby utils url link from git hub
     e.g., https://github.com/Dell-Networking/dellos10-ruby-utils/os10-devops-ruby-utils-1.0.0.deb

     <os10_devops_ruby_utils_url> - The devops ruby utils path in the switch, if previous option is local
     e.g., /home/admin/

Sample usage
------------
  ./os10_devops_infra_install.sh puppet standby_partition remote https://apt.puppetlabs.com/puppet5-release-jessie.deb local /home/admin
  (or)
  ./os10_devops_infra_install.sh puppet active_partition remote https://apt.puppetlabs.com/puppet5-release-jessie.deb remote https://github.com/Dell-Networking/dellos10-ruby-utils/os10-devops-ruby-utils-1.0.0.deb
  (or)
  ./os10_devops_infra_install.sh puppet active_partition local /home/admin/ local /home/admin/
  (or)
  ./os10_devops_infra_install.sh puppet_ruby_utils standby_partition local /home/admin
  (or)
  ./os10_devops_infra_install.sh puppet_ruby_utils active_partition remote https://github.com/Dell-Networking/dellos10-ruby-utils/os10-devops-ruby-utils-1.0.0.deb
