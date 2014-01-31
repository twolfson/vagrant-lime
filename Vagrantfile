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

  # Install dependency on `git`
  $install_git = <<SCRIPT
  if ! which git &> /dev/null; then
    sudo apt-get install git -y
  fi
SCRIPT
  config.vm.provision "shell", inline: $install_git

  # Everything so far
   #  1  sudo locale-gen en_US
   #  2  sudo apt-get install cmake -y
   #  3  sudo apt-get install make
   #  4  sudo apt-get install go
   #  5  cd /tmp/
   #  6  wget http://code.google.com/p/go/downloads/detail?name=go1.2.linux-amd64.tar.gz&can=2&q=
   #  7  wget http://go.googlecode.com/files/go1.2.linux-amd64.tar.gz
   #  8  tar xvf go1.2.linux-amd64.tar.gz
   #  9  ls
   # 10  mv go $HOME/code/go
   # 11  ls
   # 12  mkdir $HOME/code
   # 13  mv go $HOME/code/go
   # 14  ls
   # 15  cd $HOME/code/go
   # 16  export GOPATH=$HOME/code/go
   # 17  mkdir -p $GOPATH/src
   # 18  cd $GOPATH/src
   # 19  git clone --recursive https://github.com/limetext/lime.git lime
   # 20  echo $PYTHONPATH
   # 21  cd $GOPATH/src/lime/build
   # 22  cmake ..  # or use the cmake gui to create a build system suitable for you
   # 23  make      # presuming you told cmake to generate makefiles
   # 24  sudo apt-get install mercurial
   # 25  hg
   # 26  make      # presuming you told cmake to generate makefiles
   # 27  ls
   # 28  ls ..
   # 29  ls ../../
   # 30  go --version
   # 31  ../../../bin/go --version
   # 32  ../../../bin/go version
   # 33  cd /tmp/
   # 34  ls
  # It turns out we need go@1.1.2 exactly for one of the builds to work. So here we start all over again.
   # 35  rm -r ~/code/go/
   # 36  rm -rf ~/code/go/
   # 37  ls
   # 38  wget http://go.googlecode.com/files/go1.1.2.linux-amd64.tar.gz
   # 39  history
   # 40  tar xvf go1.1.2.linux-amd64.tar.gz
   # 41  mv go $HOME/code/go
   # 42  cd $HOME/code/go
   # 43  ls
   # 44  echo $GOPATH
   # 45  mkdir -p $GOPATH/src
   # 46  cd $GOPATH/src
   # 47  git clone --recursive https://github.com/limetext/lime.git lime
   # 48  cd $GOPATH/src/lime/build
   # 49  cmake ..  # or use the cmake gui to create a build system suitable for you
   # 50  make      # presuming you told cmake to generate makefiles
   # 51  make test
   # 52  make termbox
   # 53  termbox
   # 54  cd ../frontend/termbox/
   # 55  ./termbox
   # 56  history
end