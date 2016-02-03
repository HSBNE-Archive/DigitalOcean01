# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|

  required_plugins = %w( vagrant-hostmanager vagrant-pristine vagrant-triggers )
  required_plugins.each do |plugin|
    system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
  end

  if Vagrant.has_plugin?("vagrant-hostmanager") then
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.ignore_private_ip = false
    config.hostmanager.include_offline = true
  end

  config.ssh.forward_agent = true

  config.vm.define "dev.do1.hsbne.org" do |web|
    web.vm.box = "precise64"
    web.vm.network :private_network, ip: "192.168.3.3"
    web.vm.hostname = "dev.do1.hsbne.org"
    web.hostmanager.aliases = %w(dev.wiki.hsbne.org dev.forum.hsbne.org)

    web.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--memory", "2048"]
      v.customize ["modifyvm", :id, "--cpus", "2"]
      v.name = "do1"
    end

    web.vm.provider :vmware_fusion do |v|
      v.vmx["memsize"] = "2048"
      v.vmx["numvcpus"] = "2"
    end

    if Vagrant.has_plugin?("vagrant-triggers") and
      not Vagrant::Util::Platform.windows?

      web.trigger.after :up do
        run "bin/vagrant-exportsshconfig"
      end
      web.trigger.after :reload do
        run "bin/vagrant-exportsshconfig"
      end
      web.trigger.after :pristine do
        run "bin/vagrant-exportsshconfig"
      end

    end

  end

  if not Vagrant::Util::Platform.windows?
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "vagrant.yml"
      ansible.inventory_path = "./hosts"
      ansible.limit = ""
      ansible.verbose = 'vvvv'
      #ansible.start_at_task = 'Git Clone Gollum'
    end
  end
end
