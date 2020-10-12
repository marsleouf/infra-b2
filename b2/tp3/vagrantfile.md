```ru
Vagrant.configure("2")do|config|
  config.vm.box="utput"
  config.disksize.size = '10GB'

  config.vbguest.auto_update = false
  config.vm.box_check_update = false 
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.network "private_network", ip: "10.2.1.11"

  config.vm.define "tp3" do |tp3|

  config.vm.hostname = "tp3"

  config.vm.provider "virtualbox" do |vb|
     vb.memory = "1024"
     vb.name = "tp3"
     end
  end
end
```