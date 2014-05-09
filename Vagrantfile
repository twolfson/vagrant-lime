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

  # Install go
  $install_go = <<SCRIPT
  if ! test -d /vagrant/code/go &> /dev/null; then
    # Download go
    # https://code.google.com/p/go/wiki/Downloads?tm=2
    cd /tmp
    wget https://storage.googleapis.com/golang/go1.2.2.linux-amd64.tar.gz

    # Extract go and add to PATH
    sudo tar -C /usr/local -xzvf go1.2.2.linux-amd64.tar.gz
    sudo bash -c "echo 'export PATH=\$PATH:/usr/local/go/bin' >> /etc/profile"
    source /etc/profile
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_go

  # Install limetext/lime
  $build_lime = <<SCRIPT
  if ! test -d /vagrant/code/go; then
    # Define GOPATH for packages
    # http://golang.org/doc/code.html#GOPATH
    export GOPATH=/vagrant/code/go
    mkdir $GOPATH

    # Download the termbox frontend
    go get github.com/limetext/lime/frontend/termbox
    # TODO: There was an error for python3.3 and oniguruma
    # vagrant@precise64:/tmp$ go get github.com/limetext/lime/frontend/termbox
    # # pkg-config --cflags python-3.3
    # exec: "pkg-config": executable file not found in $PATH
    # # pkg-config --cflags oniguruma
    # exec: "pkg-config": executable file not found in $PATH

    # Add dependencies
    cd $GOPATH/src/github.com/limetext/lime
    git submodule update --init
  fi
SCRIPT
  config.vm.provision "shell", inline: $build_lime

  # Notify the user how to run `lime`
  $notify_user = <<SCRIPT
  echo "\\`lime\\` successfully constructed!"
  echo "To run \\`lime\\`, run the following:"
  echo "vagrant ssh"
  echo "cd /vagrant/code/go/src/lime/frontend/termbox"
  echo "./termbox"
SCRIPT
  config.vm.provision "shell", inline: $notify_user
end
