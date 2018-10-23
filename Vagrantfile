#
# Juniper lab v0.1
#
# ge-0/0/0.0: management interface
# ge-0/0/1.0 - ge-0/0/7.0: user interfaces

#default_box = 'image_to_use'
default_box = "juniper/ffp-12.1X47-D15.4-packetmode"

# Mesh Topology
#      +-------------------------------------+
#      |                                     |
# +---------+e3          e3+---------+       |
# | spine01 |--------------| spine02 |----+  |
# +---------+              +---------+    |  |
#   |e1    \ e2          e1 /    e2|      |  | 
#   |        \            /        |      |  | 
#   |          \        /          |      |  | 
#   |            \    /            |   +---------+
#   |              \/              |   | ansible |
#   |             /  \             |   +---------+
#   |           /      \           |      |  | 
#   |         /          \         |      |  | 
#   |e1     / e2        e1 \     e2|      |  |       
# +---------+              +---------+    |  |
# | leaf03  |--------------| leaf04  |----+  |
# +---------+e3          e3+---------+       |
#      |                                     |
#      +-------------------------------------+

Vagrant.configure(2) do |config|
  config.vm.box = default_box
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 2
    vb.gui = false
  end
  
  config.vm.define 'ansible' do |ansible|
	ansible.vm.box = "ubuntu/trusty64"
	ansible.vm.network :forwarded_port, guest: 22, host: 12202, id: 'ssh'
	ansible.vm.network "private_network", virtualbox__intnet: "ans_s01",
                                      ip: "10.0.1.1",
                                      netmask: "255.255.255.252",									  
									  auto_config: true
	ansible.vm.network "private_network", virtualbox__intnet: "ans_s02",
                                      ip: "10.0.1.5",
                                      netmask: "255.255.255.252",
									  auto_config: true
	ansible.vm.network "private_network", virtualbox__intnet: "ans_l03",
                                      ip: "10.0.1.9",
                                      netmask: "255.255.255.252",
									  auto_config: true	
	ansible.vm.network "private_network", virtualbox__intnet: "ans_l04",
                                      ip: "10.0.1.13",
                                      netmask: "255.255.255.252",
									  auto_config: true									  
	ansible.vm.provision "file", source: "ansible.cfg", destination: "ansible.cfg" 
    ansible.vm.provision "file", source: "provision", destination: "provision"
	ansible.vm.provision 'shell', inline: <<-SHELL
      sleep 30
	  sudo /vagrant/install.sh
    SHELL
  end
  config.vm.define 'spine01' do |spine01|
	spine01.vm.host_name = "spine01"
	spine01.vm.network :forwarded_port, guest: 22, host: 2221, id: 'ssh'
    spine01.vm.network 'private_network',
                       virtualbox__intnet: 's01_l03',
                       ip: '169.254.13.11', auto_config: false, nic_type: "virtio"
    spine01.vm.network 'private_network',
                       virtualbox__intnet: 's01_l04',
                       ip: '169.254.14.11', auto_config: false, nic_type: "virtio"
    spine01.vm.network 'private_network',
                       virtualbox__intnet: 's01_s02',
                       ip: '169.254.12.11', auto_config: false, nic_type: "virtio"
	spine01.vm.network "private_network", 
					   virtualbox__intnet: "ans_s01",
                       ip: "10.0.1.2",
                       netmask: "255.255.255.252",
					   nic_type: "virtio"
	config.vbguest.auto_update = false
  end
   config.vm.define 'spine02' do |spine02|
    spine02.vm.host_name = "spine02"
	spine02.vm.network :forwarded_port, guest: 22, host: 2222, id: 'ssh'
    spine02.vm.network 'private_network',
                       virtualbox__intnet: 's02_l03',
                       ip: '169.254.23.11', auto_config: false, nic_type: "virtio"
    spine02.vm.network 'private_network',
                       virtualbox__intnet: 's02_l04',
                       ip: '169.254.24.11', auto_config: false, nic_type: "virtio"
    spine02.vm.network 'private_network',
                       virtualbox__intnet: 's01_s02',
                       ip: '169.254.21.11', auto_config: false, nic_type: "virtio"
	spine02.vm.network "private_network", 
					   virtualbox__intnet: "ans_s02",
                       ip: "10.0.1.6",
                       netmask: "255.255.255.252",
					   nic_type: "virtio" 
	config.vbguest.auto_update = false
  end
  config.vm.define 'leaf03' do |leaf03|
    leaf03.vm.host_name = "leaf03"
	leaf03.vm.network :forwarded_port, guest: 22, host: 2223, id: 'ssh'
    leaf03.vm.network 'private_network',
                       virtualbox__intnet: 's01_l03',
                       ip: '169.254.31.11', auto_config: false, nic_type: "virtio"
    leaf03.vm.network 'private_network',
                       virtualbox__intnet: 's02_l03',
                       ip: '169.254.32.11', auto_config: false, nic_type: "virtio"
    leaf03.vm.network 'private_network',
                       virtualbox__intnet: 'l03_l04',
                       ip: '169.254.34.11', auto_config: false, nic_type: "virtio"
	leaf03.vm.network "private_network",
					   virtualbox__intnet: "ans_l03",
                       ip: "10.0.1.10",
                       netmask: "255.255.255.252",
					   nic_type: "virtio"
	config.vbguest.auto_update = false
  end

  config.vm.define 'leaf04' do |leaf04|
    leaf04.vm.host_name = "leaf04"
	leaf04.vm.network :forwarded_port, guest: 22, host: 2224, id: 'ssh'
    leaf04.vm.network 'private_network',
                       virtualbox__intnet: 's01_l04',
                       ip: '169.254.41.11', auto_config: false, nic_type: "virtio"
    leaf04.vm.network 'private_network',
                       virtualbox__intnet: 's02_l04',
                       ip: '169.254.42.11', auto_config: false, nic_type: "virtio"
    leaf04.vm.network 'private_network',
                       virtualbox__intnet: 'l03_l04',
                       ip: '169.254.43.11', auto_config: false, nic_type: "virtio"
	leaf04.vm.network "private_network",
					   virtualbox__intnet: "ans_l04",
                       ip: "10.0.1.14",
                       netmask: "255.255.255.252",
					   nic_type: "virtio"
	config.vbguest.auto_update = false
  end
end


