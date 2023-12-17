Vagrant.configure("2") do |config|
    config.vm.box = "hashicorp/bionic64"
    config.vm.provision "docker" do |d|
        d.pull_images "oldshensheep/mindustry-server:latest"
        d.run "oldshensheep/mindustry-server:latest"
  end
end