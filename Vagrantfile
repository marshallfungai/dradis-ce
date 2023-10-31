vagrant_dir = __dir__
show_logo = false
branch_c = "\033[38;5;6m" # 111m"
red = "\033[38;5;9m" # 124m"
green = "\033[1;38;5;2m" # 22m"
blue = "\033[38;5;4m" # 33m"
purple = "\033[38;5;5m" # 129m"
docs = "\033[0m"
yellow = "\033[38;5;3m" # 136m"
yellow_underlined = "\033[4;38;5;3m" # 136m"
url = yellow_underlined
creset = "\033[0m"

# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The '2' in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure('2') do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = 'ubuntu/xenial64'

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing 'localhost:8080' will access port 80 on the guest machine.
  config.vm.network 'forwarded_port', guest: 3000, host: 3000

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network 'private_network', ip: '192.168.33.10'

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network 'public_network'

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder '..', '/dradis', create: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider 'virtualbox' do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
    # Customize the amount of memory on the VM:
    vb.memory = 4096
    vb.cpus = 2
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define 'atlas' do |push|
  #   push.app = 'YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME'
  # end

   # Auto Download Vagrant plugins, supported from Vagrant 2.2.0
   unless Vagrant.has_plugin?('vagrant-goodhosts')
    if File.file?(File.join(vagrant_dir, 'vagrant-goodhosts.gem'))
      system('vagrant plugin install ' + File.join(vagrant_dir, 'vagrant-goodhosts.gem'))
      File.delete(File.join(vagrant_dir, 'vagrant-goodhosts.gem'))
      puts ("#{green} The vagrant-goodhosts plugin is now installed. Please run the requested command again.#{creset}")
      exit
    else
      config.vagrant.plugins = ['vagrant-goodhosts']
    end
  end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision 'shell', inline: <<-SHELL
    apt-get update
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
      mysql-server \
      mysql-client \
      libmysqlclient-dev \
      libfontconfig \
      libfontconfig-dev
    which phantomjs >/dev/null || ( cd /tmp && \
      wget -nv https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2 && \
      echo '86dd9a4bf4aee45f1a84c9f61cf1947c1d6dce9b9e8d2a907105da7852460d2f  phantomjs-2.1.1-linux-x86_64.tar.bz2' > phantomjs-2.1.1-linux-x86_64.tar.bz2.sha256 && \
      sha256sum -c phantomjs-2.1.1-linux-x86_64.tar.bz2.sha256
      bzip2 -d phantomjs-2.1.1-linux-x86_64.tar.bz2 && \
      tar -xf phantomjs-2.1.1-linux-x86_64.tar && \
      cp phantomjs-2.1.1-linux-x86_64/bin/phantomjs /usr/bin/phantomjs && \
      rm -rf ./phantomjs-2.1.1-linux-x86_64.*
    )
    su ubuntu -c 'type rvm >/dev/null 2>&1 || (( gpg --keyserver hkp://pgp.mit.edu --recv-keys D39DC0E3    ) && ( curl -sSL https://get.rvm.io | bash -s stable ))'
    su ubuntu -c 'cd /dradis/dradis-ce/ && source "$HOME/.profile" && rvm install "$(cat .ruby-version)"'
  SHELL

   # Auto Download Vagrant plugins, supported from Vagrant 2.2.0
    if Vagrant.has_plugin?('vagrant-goodhosts')
      puts ("#{yellow}Running vagrant goodhosts...#{creset}")
      config.vagrant.plugins = ['vagrant-goodhosts']
      config.goodhosts.aliases = ["dradis-ce.local", "dradis-dev.local"]
      config.goodhosts.remove_on_suspend = true
      config.goodhosts.disable_clean = true 
    else
      puts ("#{yellow}Running mv host name#{creset}")
      config.vm.hostname = "dradis-ce.local"
    end 
end
