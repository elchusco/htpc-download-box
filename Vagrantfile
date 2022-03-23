module VagrantVars
  
  OS = "ubuntu"

  MEMORY = 6144
  DISK = ""
  GUI = false
  HOSTNAME = 'htpc'
  TAGS = []
  SKIP_TAGS = []
  VERBOSE = false
  SHELL_PROVISION = true

end

include VagrantVars

Vagrant.configure("2") do |config|

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "#{MEMORY}"
    unless DISK.empty?
      config.disksize.size = "#{DISK}"
    end
  end
  
  config.vm.box = "ubuntu/focal64"
  config.vm.network "private_network", ip: "192.168.56.12"

  unless HOSTNAME.empty?
    config.vm.hostname = "#{HOSTNAME}"
    config.vm.provision "shell", inline: "hostname #{HOSTNAME} && hostname > /etc/hostname"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.host_key_checking = false
    ansible.playbook = "ansible/playbook.yml"
    ansible.extra_vars = {
      is_vagrant: true,
    }
    unless HOSTNAME.empty?
      ansible.extra_vars['hostname'] = "#{HOSTNAME}"
    end
    if TAGS.any?
      ansible.tags = TAGS
    end
    if SKIP_TAGS.any?
      ansible.skip_tags = SKIP_TAGS
    end
    if VERBOSE
      ansible.verbose = "vvv"
    end
    ansible.compatibility_mode = "2.0"
  end

end
