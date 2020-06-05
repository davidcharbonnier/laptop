Vagrant.configure("2") do |config|
  config.vm.box = "generic/fedora32"
  
  # Set VM resources
  config.vm.provider :libvirt do |lv|
    lv.cpus = 4
    lv.memory = 8192
  end

  # Disable sync folder
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Set VM settings
  config.vm.define vm_name = "laptop"
  config.vm.hostname = "laptop.lab.local"
  ip = "192.168.250.10"
  config.vm.network :private_network, ip: ip

  # Launch Ansible playbook
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yml"
    # "pass" tag need GPG key inserted that cannot be achieved in Vagrant
    ansible.skip_tags = "pass"
  end
end
