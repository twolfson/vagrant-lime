# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Update apt-get once
  $update_apt_get = <<SCRIPT
  if ! test -f .updated_apt_get; then
    sudo apt-get update
    touch .updated_apt_get
  fi
SCRIPT
  config.vm.provision "shell", inline: $update_apt_get

  # Install dependencies
  $install_git = <<SCRIPT
  if ! which git &> /dev/null; then
    # mercurial is for one of the dependencies inside of make
    # build-essential contains `g++` which is for `cmake`
    sudo apt-get install cmake make mercurial git build-essential -y
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_git

  # Download go
  $download_go = <<SCRIPT
  if ! test -d /home/vagrant/code/go &> /dev/null; then
    cd /tmp
    wget http://go.googlecode.com/files/go1.1.2.linux-amd64.tar.gz
    tar xvf go1.1.2.linux-amd64.tar.gz
    mkdir /home/vagrant/code
    mv go /home/vagrant/code/go
    chown -R vagrant:vagrant /home/vagrant/code
  fi
SCRIPT
  config.vm.provision "shell", inline: $download_go

  # Clone and make the repo
  $build_lime = <<SCRIPT
  if ! test -d /home/vagrant/code/go/src; then
    export GOPATH=/home/vagrant/code/go
    mkdir -p $GOPATH/src
    cd $GOPATH/src
    git clone --recursive https://github.com/limetext/lime.git lime
    cd $GOPATH/src/lime/build
    cmake ..
    make
    make termbox
    chown -R vagrant:vagrant /home/vagrant/code
  fi
SCRIPT
  config.vm.provision "shell", inline: $build_lime

  # Notify the user how to run `lime`
  $notify_user = <<SCRIPT
  if ! which /home/vagrant/code/go/src &> /dev/null; then
    echo "\\`lime\\` successfully constructed!"
    echo "To run \\`lime\\`, run the following:"
    echo "vagrant ssh"
    echo "cd /home/vagrant/code/go/src/lime/frontend/termbox"
    echo "./termbox"
  fi
SCRIPT
  config.vm.provision "shell", inline: $notify_user
end
