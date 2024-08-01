# -*- mode: ruby -*-
# vi: set ft=ruby :
#ENV["VAGRANT_EXPERIMENTAL"] = "disks"
 
boxes = [
    {
        :name => "master",
        :eth1 => "192.168.56.14",
        :mem => "2048",
        :cpu => "1"
    },
    {
        :name => "worker1",
        :eth1 => "192.168.56.15",
        :mem => "2048",
        :cpu => "1"
    },
    {
        :name => "worker2",
        :eth1 => "192.168.56.16",
        :mem => "2048",
        :cpu => "1"
    }
]
 
 
Vagrant.configure("2") do |config|
 
  config.vm.provider :"virtualbox"
  config.vm.box = "gr-rocky8"
  config.vm.box_url = "https://artifactory.globalrelay.net:443/artifactory/api/vagrant/vagrant-at/gr-rocky8/1.0.0/virtualbox-rocky8.5.box"
 
  
 boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      disk_name = opts[:name]
 
      config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--name", opts[:name]]
        v.customize ["modifyvm", :id, "--memory", opts[:mem]]
        v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
        data1_disk = "#{disk_name}_data1_disk.vdi"
        unless File.exist?(data1_disk)
          # 80 GB
          v.customize ['createhd', '--filename', data1_disk, '--size', 80 * 1024]
        end
        v.customize ['storageattach', :id, '--storagectl', 'SATA', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', data1_disk]
      end
      config.vm.network :private_network, ip: opts[:eth1]
    end
  end
 
end
