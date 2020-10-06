```php
Vagrant.configure("2") do |config|
  config.vm.box = "utput"
  config.vbguest.auto_update = false
  config.vm.box_check_update = false 

  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.define "node1" do |node1|
    node1.vm.network "private_network", ip: "192.168.2.21"
    node1.vm.hostname = "node1.tp2.b2"
    node1.vm.provider "node1" do |n1|
      n1.vm.memory = "1024"
    end
  end

  config.vm.define "node2" do |node2|
    node2.vm.network "private_network", ip: "192.168.2.22"
    node2.vm.hostname = "node2.tp2.b2"
    node2.vm.provider "node2" do |n2|
      n2.vm.memory = "512"
    end
  end

  config.vm.provision :shell, path: "script.sh", run: 'always'
end
```