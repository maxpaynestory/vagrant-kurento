# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"

  config.vm.hostname = "kurento.maxpaynestory.dev"

  config.vm.network "forwarded_port", guest: 8888, host: 8888
  config.vm.network "forwarded_port", guest: 8443, host: 8443

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  #//cdn.temasys.com.sg/adapterjs/0.13.x/adapter.min.js
  #http-server -p 8443

  $rootscript = <<-EOS
    echo "deb http://ubuntu.kurento.org trusty kms6" | tee /etc/apt/sources.list.d/kurento.list
    wget -O - http://ubuntu.kurento.org/kurento.gpg.key | apt-key add -
    apt-get update -y
    curl -sL https://deb.nodesource.com/setup_4.x | bash -
    apt-get install -y nodejs
    npm install -g bower forever http-server
    apt-get install -y git
    export DEBIAN_FRONTEND=noninteractive
    apt-get install -y kurento-media-server-6.0
    service kurento-media-server-6.0 start
  EOS

  $unprivileged = <<-EOS
    cd /vagrant
    mkdir repos
    cd repos
    git clone https://github.com/maxpaynestory/kurento-tutorial-js
    cd kurento-tutorial-js/kurento-recorder
    git checkout 6.2.1
    rm bower.json
    bower install adapter.js bootstrap ekko-lightbox demo-console kurento-client kurento-utils
  EOS

  config.vm.provision "shell", inline: $rootscript

  config.vm.provision "shell", inline: $unprivileged, privileged: false

end
