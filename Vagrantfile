IMAGE_NAME = "oraclebase/oracle-7"

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 1
    end
      
    (1..3).each do |i|
      config.vm.define "master-#{i}" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "public_network", ip: "192.168.0.#{i + 10}", bridge: "wlx90de8012d526"
        master.vm.hostname = "k8s-master"
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "kubernetes-setup/master-playbook.yml"
            ansible.extra_vars = {
                node_ip: "192.168.0.#{i + 10}",
            }
        end
    end
  end
    (1..2).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "public_network", ip: "192.168.0.#{i + 14}"
            node.vm.hostname = "node-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "kubernetes-setup/node-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "192.168.0.#{i + 10}",
                }
            end
        end
    end
end