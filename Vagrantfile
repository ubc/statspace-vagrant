# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "puppetlabs/centos-7.2-64-nocm"

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true#  if Vagrant.has_plugin?('landrush')
#    ## allow direct access by hostname
#    config.landrush.enabled = true
#    # add my public key (without overwriting the key already added by Vagrant)
#    config.vm.provision :file, source: "~/.ssh/id_rsa.pub", destination: "/tmp/hostkey"
#    config.vm.provision "shell", inline: 'cat /tmp/hostkey >> ~vagrant/.ssh/authorized_keys && rm /tmp/hostkey'
#  end

  config.vm.hostname = "statspace.vagrant.test"

  # keep the source in a shared folder so I can build locally for faster deployment
  config.vm.synced_folder "dspace", "/var/dspace-src"

  # Access to Tomcat running on the VM
  config.vm.network "forwarded_port", guest: 8080, host: 8080

  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end

  # copy ~/.inputrc
  #config.vm.provision :file, source: '~/.inputrc', destination: '.inputrc'

  # Set up better time-keeping
  config.vm.provision "shell", inline: <<-SHELL
     timedatectl set-timezone America/Vancouver
     timedatectl set-local-rtc 0
     yum install -y chrony git
     systemctl start chronyd
  SHELL

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = 'site.yml'
    ansible.inventory_path = 'inventory/vagrant.ini'
    ansible.limit = 'all'
    ansible.verbose = true
    ansible.skip_tags = ['checkout']
    ansible.galaxy_role_file = 'requirements.yml'
  end
end
