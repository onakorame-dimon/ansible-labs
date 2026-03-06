
Vagrant.configure("2") do |config|

  config.vm.box = "hashicorp-education/ubuntu-24-04"
  config.vm.box_version = "0.1.0"
  config.vm.box_check_update = false
 
  config.vm.provider "virtualbox" do |v|
    v.linked_clone = true
    v.memory = 512
    end

 # Node server
  config.vm.define "node-01" do | node |
    node.vm.hostname = "node-01"
    node.vm.network  "private_network", ip: "10.0.0.11"
    node.vm.synced_folder ".", "/vagrant", disabled: true
  end    

 # controller Server
  config.vm.define "controller" do |controller|
    controller.vm.hostname = "controller"
    controller.vm.network "private_network", ip: "10.0.0.10"

    # configure key authentication for controller to node-01
    $script = <<-'SCRIPT'
    touch /home/vagrant/.ssh/node-01
    cp -v /vagrant/.vagrant/machines/node-01/virtualbox/private_key /home/vagrant/.ssh/node-01
    chmod 600 /home/vagrant/.ssh/node-01
    chown vagrant:vagrant /home/vagrant/.ssh/node-01

    touch /home/vagrant/.ssh/config 
    chmod 600 /home/vagrant/.ssh/config 
    chown vagrant:vagrant /home/vagrant/.ssh/config 
    cat <<-EOF > /home/vagrant/.ssh/config
      Host node-01
      HostName 10.0.0.11
      User vagrant
      IdentityFile /home/vagrant/.ssh/node-01
EOF
    SCRIPT
  controller.vm.provision "shell", inline: $script
  end

end


