Vagrant.configure("2") do |config|
  config.vm.define :t3ansible do |t3ansible|

    t3ansible.vm.box = "t3ansible-ubuntu"
    t3ansible.vm.box_url = "/Users/mimi/Workspace/vagrant_boxes/ubuntu-14.04-amd64.box"

    t3ansible.vm.provider "virtualbox" do |v|
      v.name = "T3Ansible-Ubuntu"
      v.gui = true
      v.customize [
                    "modifyvm", :id,
                    "--memory", 2048,
                    "--cpus", 2,
                    "--natdnshostresolver1", "on"
                  ]
    end

    t3ansible.vm.synced_folder ".", "/vagrant", id: "vagrant-root", disabled: true

    t3ansible.vm.hostname = "t3ansible.t3.dev.punkt.de"
    t3ansible.vm.network :private_network, ip: "10.20.30.100", netmask: "255.255.255.0"

    t3ansible.ssh.forward_agent = true

  end
end
