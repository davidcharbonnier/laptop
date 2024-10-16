Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2204"
  # a few things are not yet fully compatible with redhat based distros (package names, some tools)
  #config.vm.box = "generic/fedora39"
  
  # Select libvirt provider
  config.vm.provider "libvirt"

  config.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [ ".git/", "venv/", ".vagrant/" ]

  # Set VM settings
  config.vm.define vm_name = "laptop"

  # Install/remove some system dependencies which conflicts or cannot be met with pip ones
  config.vm.provision "shell", inline: <<-SCRIPT
  if [ $(which dnf &>/dev/null; echo $?) -eq 0 ]
  then
    sudo dnf remove -y python3-pip
    sudo dnf install -y python3-dnf
  elif [ $(which apt &>/dev/null; echo $?) -eq 0 ]
  then
    sudo DEBIAN_FRONTEND=noninteractive apt-get update
    sudo DEBIAN_FRONTEND=noninteractive apt-get install -q -y python3-apt
  fi
  SCRIPT

  # Launch Ansible playbook
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "site.yml"
    # install galaxy dependencies
    ansible.galaxy_role_file = "requirements.yml"
    # install ansible using pip requireemnts.txt file, force install each time with "latest" version
    ansible.version = "latest"
    ansible.install_mode = "pip_args_only"
    ansible.pip_install_cmd = "curl https://bootstrap.pypa.io/get-pip.py | sudo python3"
    ansible.pip_args = "-r /vagrant/requirements.txt --retries=60 --timeout=60"
    # force ansible to use python3 interpreter (required on RedHat family)
    ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
  end
end
