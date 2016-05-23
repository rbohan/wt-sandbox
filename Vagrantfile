# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
    # Install Centos 7.1
    config.vm.box = "centos/7"

    # Server VM
    config.vm.define :server do |server|
        server.vm.provider "virtualbox" do |vb|
            vb.name = "wtserver"
            vb.memory = 4096
            #vb.memory = 2048

            # Get disk path
            line = `VBoxManage list systemproperties | grep "Default machine folder"`
            vb_machine_folder = line.split(':')[1].strip()
            disk2 = File.join(vb_machine_folder, vb.name, 'wtdisk2.vdi')
            disk3 = File.join(vb_machine_folder, vb.name, 'wtdisk3.vdi')

            # Create and attach disks
            sizeInGB = 2 * 1024

            unless File.exist?(disk2)
              vb.customize ['createhd', '--filename', disk2, '--format', 'VDI', '--variant', 'Fixed', '--size', sizeInGB]
            end
            vb.customize ['storageattach', :id, '--storagectl', 'IDE Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', disk2]

            unless File.exist?(disk3)
              vb.customize ['createhd', '--filename', disk3, '--format', 'VDI', '--variant', 'Fixed', '--size', sizeInGB]
            end
            vb.customize ['storageattach', :id, '--storagectl', 'IDE Controller', '--port', 1, '--device', 1, '--type', 'hdd', '--medium', disk3]
        end

        server.vm.hostname = "wtserver.vagrant.dev"
        server.vm.network :private_network, ip: "192.168.14.201"

        #server.vm.synced_folder "server/", "/home/vagrant"

        server.vm.provision :ansible do |ansible|
            ansible.playbook = "wtserver.yml"
        end
    end

    # Client VM
    config.vm.define :client do |client|
        client.vm.provider "virtualbox" do |vb|
            vb.name = "wtclient"
            vb.memory = 512
        end
        client.vm.hostname = "wtclient.vagrant.dev"
        client.vm.network :private_network, ip: "192.168.14.200"

        #client.vm.synced_folder "client/", "/home/vagrant"

        client.vm.provision :ansible do |ansible|
            ansible.playbook = "wtclient.yml"
        end
    end
=begin
=end
end
