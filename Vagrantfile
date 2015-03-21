=begin

This script runs as a provisioner and

* creates a sudo-enabled user 'ansible' with password 'ansible' which you can use as a user for running Ansible playbooks
* copies a `.my.cnf` file into the home directory of this user
* installs MySQL server
* imports a dummy database with some sample data for testing

=end

$script = <<SCRIPT
echo Provisioning Box for Testing with Ansible...
sudo useradd -m -s /bin/bash -p does_not_matter_set_it_later -G sudo ansible
sudo echo -e \"ansible\nansible\" | (sudo passwd ansible)
sudo cp /vagrant/fixtures/my.cnf /home/ansible/.my.cnf
sudo chown ansible /home/ansible/.my.cnf
sudo echo 'mysql-server mysql-server/root_password password root' | sudo debconf-set-selections
sudo echo 'mysql-server mysql-server/root_password_again password root' | sudo debconf-set-selections
sudo apt-get update
sudo apt-get install mysql-server -y --assume-yes
mysql -u root -proot < /vagrant/fixtures/typo3org.sql
SCRIPT



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
    t3ansible.vm.synced_folder ".", "/vagrant", :nfs => true, :nfs_version => 3

    t3ansible.vm.hostname = "t3ansible.t3.dev.punkt.de"
    t3ansible.vm.network :private_network, ip: "10.20.30.100", netmask: "255.255.255.0"

    t3ansible.ssh.forward_agent = true

    t3ansible.vm.provision "shell", inline: $script

  end
end
