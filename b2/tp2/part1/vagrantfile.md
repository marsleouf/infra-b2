```go
Vagrant.configure("2")do|config|
  config.vm.box="centos/7"
  config.disksize.size = '10GB'

  config.vbguest.auto_update = false
  config.vm.box_check_update = false 
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.network "private_network", ip: "192.168.2.11"

  config.vm.define "hello"

  config.vm.hostname = "hello"

  config.vm.provider "virtualbox" do |vb|
     vb.memory = "1024"
     vb.name = "hello"
     end

  config.vm.provision :shell, path: "script.sh", run: 'always'

  file_to_disk = 'VagrantDisk.vdi'

  config.vm.provider "virtualbox" do |vb|
    vb.customize ['createhd', '--filename', file_to_disk, '--size', 5120]
    vb.customize ['storageattach', :id, '--storagectl', 'IDE', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
  end
end
```