## programa para compartir consola via web

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'
IMAGEN = "generic/ubuntu2004"

$script = <<-SCRIPT
sudo apt-get update && sudo apt-get -y install build-essential cmake git libjson-c-dev libwebsockets-dev
git clone https://github.com/tsl0922/ttyd.git 
mkdir -p ttyd/build && cd ttyd/build
cmake ..
make && sudo make install
SCRIPT

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", type: "rsync"
  
  config.vm.define :server do |s|
    s.vm.box = IMAGEN
    s.vm.hostname = "ttyd"
    s.vm.box_check_update = false
    s.vm.network "forwarded_port", guest: 9000, host: 9001
    s.vm.provision "shell", inline: $script

    s.vm.provider :libvirt do |v|
      v.memory = 1024
      v.cpus = 2
    end
  end
end

## ttyd -p 9000 -c pgraffigna:test123 bash