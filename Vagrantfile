# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  config.vm.define :jenkins do |jenkins|
    jenkins.vm.hostname = "ubuntu14"
    jenkins.vm.box = "opscode-ubuntu-14.04"
    jenkins.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_ubuntu-14.04_chef-provisionerless.box"
    jenkins.vm.network :private_network, ip: "192.168.39.10"
    jenkins.vm.network :forwarded_port, host: 8022, guest: 22

    jenkins.vm.synced_folder "ansible", "/home/vagrant/ansible",
     id: "vagrant-root", :nfs => false,
     :owner => "vagrant",
     :group => "www-data",
     :mount_options => ["dmode=775,fmode=664"]

    jenkins.vm.provision "shell", inline: <<-SHELL
      apt-get -y install ansible
      cd /vagrant/ansible; ansible-playbook -i hosts/localhost vagrant.yml
    SHELL
    
#    jenkins.vm.provision :ansible do |ansible|
#      ansible.limit = 'all'
#      ansible.inventory_path = "ansible/hosts/vagrant-hosts"
#      ansible.playbook = "ansible/baseball-devel.yml"
#    end
  end
end