Vagrant.require_version ">= 2.0.0"

unless Vagrant.has_plugin?("vagrant-disksize")
  raise 'vagrand-disksize plugin for vagrand has to be installed!'
end


Vagrant.configure("2") do |config|

  
  config.vm.define "master" do |master|
    master.vm.box = "centos/8"
    master.vm.network "private_network", type: "dhcp"
    master.vm.hostname = "master.vm.local"
    master.disksize.size = "50GB"
    

  end

  config.vm.define "slave" do |slave|
    slave.vm.box = "centos/8"
    slave.vm.network "private_network", type: "dhcp"
    slave.vm.hostname = "slave.vm.local"
    slave.disksize.size = "50GB"


  end

  #Shell provisioning
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/aut.pub"
  config.vm.provision "shell", inline: "cat ~vagrant/.ssh/aut.pub >> ~vagrant/.ssh/authorized_keys"
  
  #Ansible provisioning
  config.vm.provision "ansible" do |ansible|
    
    ansible.limit = "all"

    ansible.groups = {
      "swarm-master" => ["master"],
      "swarm-workers" => ["slave"],
      "nodes" => ["master", "slave"]
    }

    ansible.playbook = "provisioning/playbook.yml"
  end


end
