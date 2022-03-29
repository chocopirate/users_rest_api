# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/bionic64"
    config.ssh.insert_key = false
    config.vbguest.auto_update = false
#    config.vm.box_version = "~> 20200304.0.0"
    config.vm.network "forwarded_port", guest: 9999, host: 9999, auto_correct: true
# config.vm.provider "virtualbox" do |vb|
#     config.vm.network "private_network", :type => 'dhcp', :adapter => 3
# end
    config.vm.network :public_network, :bridge => 'en1: Wi-Fi (AirPort)'
    config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--uart1", "0x3F8", "4"]
        vb.customize ["modifyvm", :id, "--uartmode1", "file", File::NULL]
#         vb.name = "profiles_rest_api_host"
    end

    config.vm.provision "shell", inline: <<-SHELL
        systemctl disable apt-daily.service
        systemctl disable apt-daily.timer

        sudo apt-get update
        sudo apt-get install -y python3-venv zip
        touch /home/vagrant/.bash_aliases
        if ! grep -q PYTHON_ALIAS_ADDED /home/vagrant/.bash_aliases; then
            echo "# PYTHON_ALIAS_ADDED" >> /home/vagrant/.bash_aliases
            echo "alias python='python3'" >> /home/vagrant/.bash_aliases
        fi
    SHELL
end
