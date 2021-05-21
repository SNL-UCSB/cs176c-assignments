# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure(2) do |config|

  # 64 bit Ubuntu Vagrant Box
  config.vm.box = "ubuntu/xenial64"

  ## Configure hostname and port forwarding
  config.vm.hostname = "cs176c"
  config.ssh.forward_x11 = true
  config.vm.network "forwarded_port", guest: 8888, host: 8888

  vagrant_root = File.dirname(__FILE__)

  # Jupyter notebook settings
  config.vm.provision "file", source: "#{vagrant_root}/config_files/jupyter_notebook_config.py", destination: "~/.jupyter/jupyter_notebook_config.py"

  ## Provisioning
  config.vm.provision "shell", inline: <<-SHELL
     sudo add-apt-repository -y ppa:deadsnakes/ppa
     sudo apt-get update
     sudo apt-get -y upgrade

     sudo apt-get -y install python3.6-dev
     curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
     python3.6 get-pip.py
     rm get-pip.py

     echo "export PYTHONPATH=${PYTHONPATH}:/vagrant/course-bin" >> /home/vagrant/.profile

     # Set correct permissions for bash scripts
     # find /vagrant -name "*.sh" | xargs chmod -v 744

     # If the repository was pulled from Windows, convert line breaks to Unix-style
     sudo apt-get install -y dos2unix
     printf "Using dos2unix to convert files to Unix format if necessary..."
     find /vagrant -name "*" -type f | xargs dos2unix -q

     # Bufferbloat
     sudo apt-get install -y mininet
     # pip3 install jupyter nbconvert mininet numpy tzupdate

     # Start in /vagrant instead of /home/vagrant
     if ! grep -Fxq "cd /vagrant" /home/vagrant/.bashrc
     then
      echo "cd /vagrant" >> /home/vagrant/.bashrc
     fi
  SHELL

  ## Provisioning to do on each "vagrant up"
  config.vm.provision "shell", run: "always", inline: <<-SHELL
    sudo tzupdate 2> /dev/null
    # Bufferbloat
    sudo modprobe tcp_probe port=5001 full=1
  SHELL

  ## CPU & RAM
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
    vb.memory = 2048
    vb.cpus = 1
  end
end
