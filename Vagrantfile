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

  # Add Python3.3
  $install_python3 = <<SCRIPT
  if ! which python3.3 &> /dev/null; then
    # https://github.com/limetext/lime/blob/dbd2d9f6d0ea3f28b763b40c7d505d03570bd779/.travis.yml#L6-L8
    sudo apt-get install python-software-properties -y
    echo 'yes' | sudo add-apt-repository ppa:fkrull/deadsnakes
    sudo apt-get update
    sudo apt-get install python3.3 python3.3-dev -y
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_python3

  # Install dependencies
  $install_dependencies = <<SCRIPT
  if ! which git &> /dev/null; then
    # mercurial is for one of the dependencies inside of make
    # build-essential contains `g++` which is for `cmake`
    # libonig-dev is a dependency for `limetext/lime`
    # pkg-config is a dependency for `go get`
    sudo apt-get install cmake make mercurial git build-essential libonig-dev pkg-config -y

    # Add patch for oniguruma
    cd /usr/lib/pkgconfig
    sudo wget https://github.com/limetext/rubex/blob/ecf1f23794e3230cc857cf4dab44af3a2af04c46/oniguruma.pc
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_dependencies

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
  $install_lime = <<SCRIPT
  if ! test -d /vagrant/code/go; then
    # Define GOPATH for packages
    # http://golang.org/doc/code.html#GOPATH
    export GOPATH=/vagrant/code/go
    mkdir -p $GOPATH

    # Download the termbox frontend (installs to $GOPATH)
    go get github.com/limetext/lime/frontend/termbox

    # Add dependencies
    cd $GOPATH/src/github.com/limetext/lime
    git submodule update --init

    # Build the frontend
    cd $GOPATH/src/github.com/limetext/lime/frontend/termbox
    go build
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_lime

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
