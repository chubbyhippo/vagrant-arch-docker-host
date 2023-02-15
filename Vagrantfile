Vagrant.configure("2") do |config|
    config.vm.box = "generic/arch"
    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 1
    end
    config.vm.synced_folder ".", "/home/vagrant/dev"
    config.vm.network "forwarded_port", guest: 2375, host: 2375
    config.vm.provision "shell", reboot: true, inline: <<-SHELL
        sudo pacman -Syu
        sudo pacman -S --noconfirm docker
        sudo usermod -aG docker vagrant
        sudo mkdir /etc/systemd/system/docker.service.d
        echo '[Service]' | sudo tee -a /etc/systemd/system/docker.service.d/docker.conf
        echo 'ExecStart=' | sudo tee -a /etc/systemd/system/docker.service.d/docker.conf
        echo 'ExecStart=/usr/bin/dockerd -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2375' | sudo tee -a /etc/systemd/system/docker.service.d/docker.conf
        sudo systemctl enable docker.service
    SHELL
end
