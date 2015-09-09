require 'socket'
Vagrant.configure("2") do |config|
  config.vm.hostname = Socket.gethostname + '.vagrant'
  config.vm.box = 'docker-14'
  config.vm.box_url = "https://github.com/jose-lpa/packer-ubuntu_14.04/releases/download/v2.0/ubuntu-14.04.box"
  config.vm.provision :shell, :inline => $BOOTSTRAP_SCRIPT # see below
  config.ssh.forward_agent = true
end

$BOOTSTRAP_SCRIPT = <<EOF
  set -e # Stop on any error
  export DEBIAN_FRONTEND=noninteractive

  # Pre-reqs
  sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
  apt-get install -y wget ca-certificates
  wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
  apt-get update
  apt-get install -y python-software-properties make git-core curl g++ libncurses-dev texinfo groff gettext

  # Build
  cd /vagrant && ./configure && make
  cd /vagrant && mv src/nano .

  # Go to /vagrant
  echo "cd /vagrant" | sudo tee -a ~vagrant/.profile

  # Fix perms
  chown -R vagrant /home/vagrant/

  echo VAGRANT IS READY.
EOF
