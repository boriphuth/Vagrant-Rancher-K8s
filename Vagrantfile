IMAGE_NAME = "ubuntu/bionic64"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    # Virtualbox configuration
    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
    end
    
     # Rancher configuration
     config.vm.define "rancher" do |rancher|
        rancher.vm.box = IMAGE_NAME
        rancher.vm.network "public_network", ip: "192.168.1.200"
        rancher.vm.hostname = "rancher"

        # Virtualbox configurations
        rancher.vm.provider "virtualbox" do |v|
            v.cpus = 2
        end
    end

    # K8s master configuration
    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "public_network", ip: "192.168.1.201"
        master.vm.hostname = "k8s-master"

        # Virtualbox configurations
        master.vm.provider "virtualbox" do |v|
            v.cpus = 2
        end
    end

    # K8s worker configuration
    (1..N).each do |i|
        config.vm.define "k8s-node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "public_network", ip: "192.168.1.#{i + 201}"
            node.vm.hostname = "k8s-node-#{i}"
        end
    end

    config.vm.provision "ansible_local" do |ansible|
        ansible.config_file = "ansible.cfg"
        ansible.playbook = "playbook.yml"
        ansible.groups = {
            rancher: ["rancher"],
            masters: ["k8s-master"],
            nodes: ["k8s-node-1"]
            # nodes: ["k8s-node-1","k8s-node-2"]
            
        }
    end

end