IMAGE_NAME = "debian/buster64"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
        v.gui = true
    end
      
    config.vm.define "master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "10.0.3.10"
        master.vm.hostname = "master"
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "kubernetes-setup/master-playbook.yml"
            ansible.extra_vars = {
                node_ip: "10.0.3.10",
            }
        end
    end

    (1..3).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "10.0.3.#{i + 20}"
            node.vm.hostname = "node-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "kubernetes-setup/node-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "10.0.3.#{i + 20}",
                }
            end
        end
    end
end
