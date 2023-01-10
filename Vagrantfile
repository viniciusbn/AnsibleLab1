Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.provider "virtualbox" do |virtualbox|
    virtualbox.memory = 512
    virtualbox.cpus = 1
  end

  config.vm.define "ansible" do |ansible|
    ansible.vm.provider "virtualbox" do |virtualbox|
      #virtualbox.memory = 1024
      #virtualbox.cpus = 2
      virtualbox.name = "AnsibleController"
    end
    $script_ansyble = <<-SCRIPT
      apt update && \
      apt install python3-distutils python3-apt -y && \
      curl https://bootstrap.pypa.io/pip/3.6/get-pip.py -o get-pip.py && \
      python3 get-pip.py && \
      python3 -m pip install ansible
    SCRIPT
    ansible.vm.network "private_network", ip: "192.168.20.10"
    ansible.vm.synced_folder "./configs", "/configs"
    ansible.vm.provision "shell", inline: $script_ansyble
    ansible.vm.provision "shell", inline: "cat /configs/ssl/id_bionic.pub >> .ssh/authorized_keys"
    ansible.vm.provision "shell",
      inline: "cp /configs/ssl/id_bionic /home/vagrant && \
              chmod 600 /home/vagrant/id_bionic && \
              chown vagrant:vagrant /home/vagrant/id_bionic"
    #ansible.vm.provision "shell", inline: "ansible-playbook -i /configs/ansible/hosts /configs/ansible/playbook.yml"
  end

  config.vm.define "wordpress_web" do |wordpress_web|
    wordpress_web.vm.provider "virtualbox" do |virtualbox|
      #virtualbox.memory = 1024
      #virtualbox.cpus = 2
      virtualbox.name = "wordpress_web"
    end
    wordpress_web.vm.network "private_network", ip: "192.168.20.20"
    wordpress_web.vm.synced_folder "./configs", "/configs"
    wordpress_web.vm.provision "shell", inline: "cat /configs/ssl/id_bionic.pub >> .ssh/authorized_keys"
  end
  
  config.vm.define "wordpress_db" do |wordpress_db|
    wordpress_db.vm.provider "virtualbox" do |virtualbox|
      #virtualbox.memory = 1024
      #virtualbox.cpus = 2
      virtualbox.name = "wordpress_db"
    end
    wordpress_db.vm.network "private_network", ip: "192.168.20.30"
    wordpress_db.vm.synced_folder "./configs", "/configs"
    wordpress_db.vm.provision "shell", inline: "cat /configs/ssl/id_bionic.pub >> .ssh/authorized_keys"
  end
end