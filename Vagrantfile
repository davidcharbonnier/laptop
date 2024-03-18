Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2204"
  
  # Set VM resources
  config.vm.provider :libvirt do |lv|
    lv.cpus = 4
    lv.memory = 8192
  end

  # Set VM settings
  config.vm.define vm_name = "laptop"

  # Launch Ansible playbook
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yml"
    # "pass" tag need GPG key inserted that cannot be achieved in Vagrant
    ansible.skip_tags = "pass"
    # generate a new format for ansible inventory
    ansible.compatibility_mode = "2.0"
  end
end
