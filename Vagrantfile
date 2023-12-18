Vagrant.configure("2") do |config|
  config.vm.define "filegator" do |filegator|
    filegator.vm.box = "bento/ubuntu-18.04"
    filegator.vm.network "private_network", ip: "192.168.50.10"
    filegator.vm.network "forwarded_port", guest: 80, host: 80
    filegator.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
      vb.customize ["modifyvm", :id, "--uart1", "0x3F8", "4"]
      vb.customize ["modifyvm", :id, "--uartmode1", "file", File::NULL]
      vb.gui = true
      vb.memory = "2048"
      vb.cpus = 2
    end
    filegator.vm.provision "shell", inline: <<-SHELL
      sudo apt update
      sudo apt install -y wget unzip php apache2 libapache2-mod-php php-zip php-mbstring php-dom php-xml
      cd /var/www/
      wget https://github.com/filegator/static/raw/master/builds/filegator_latest.zip
      sudo unzip filegator_latest.zip && rm filegator_latest.zip
      sudo chown -R www-data:www-data filegator/
      chmod -R 775 filegator/
      sudo echo "
      <VirtualHost *:80>
          DocumentRoot /var/www/filegator/dist
      </VirtualHost>
      " >> /etc/apache2/sites-available/filegator.conf
      sudo a2dissite 000-default.conf
      sudo a2ensite filegator.conf
      sudo systemctl restart apache2
    SHELL
  end
end