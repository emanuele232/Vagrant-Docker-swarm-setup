Vagrant.require_version ">= 2.0.0"

unless Vagrant.has_plugin?("vagrant-disksize")
  raise 'vagrand-disksize plugin for vagrand has to be installed!'
end


Vagrant.configure("2") do |config|


  config.vm.box = 'centos/8'

  config.vm.provision 'file',
    source: '~/.ssh/id_rsa.pub',
    destination: '/home/vagrant/pubkey'

  config.vm.provision 'shell',
    run: 'once',
    inline: <<-SHELL
      mkdir -p /root/.ssh/
      rm -f /root/.ssh/authorized_keys
      cp /home/vagrant/pubkey /root/.ssh/authorized_keys
      cp /home/vagrant/pubkey /home/vagrant/.ssh/authorized_keys
      rm -f /home/vagrant/pubkey
      SHELL
  

  {
    'master' => '192.168.33.10',
    'slave' => '192.168.33.11'

  }.each do |name, ip|
    config.vm.define name do |host|

      host.vm.network 'private_network', ip: ip
      host.vm.hostname = "#{name}.myapp.dev"

    end
  end
end
