Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-22.04"
  config.vm.hostname = "ApplicationServer"
  config.vm.network "private_network", ip: "192.168.50.10"
  config.vm.network "forwarded_port", guest: 3000, host: 3000
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 27960, host: 27960
  config.vm.network "forwarded_port", guest: 8888, host: 8888
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.vm.network "forwarded_port", guest: 8000, host: 8000
  config.vm.network "forwarded_port", guest: 5432, host: 5432
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
    vb.customize ["modifyvm", :id, "--uart1", "0x3F8", "4"]
    vb.customize ["modifyvm", :id, "--uartmode1", "file", File::NULL]
    vb.gui = true
    vb.memory = "8192"
    vb.cpus = 3
  end
  config.vm.provision "docker" do |d|
    d.pull_images "amir20/dozzle:latest"
    d.run "amir20/dozzle:latest", args: "-d --volume=/var/run/docker.sock:/var/run/docker.sock -p 8888:8080"
    d.pull_images "filegator/filegator"
    d.run "filegator/filegator", args: "-p 5000:8080"
    d.pull_images "postgres"
    d.run "postgres", args: "-e POSTGRES_PASSWORD=postgres -e POSTGRES_USER=postgres -e POSTGRES_DB=postgres -p 5432:5432"
    d.pull_images "delsynn/reactle:latest"
    d.run "delsynn/reactle:latest", args: "-d -p 3000:3000"
    d.pull_images "adamboutcher/statping-ng"
    d.run "adamboutcher/statping-ng", args: "-it -p 8000:8080"
  end
end