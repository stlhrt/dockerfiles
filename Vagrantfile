# -*- mode: ruby -*-
# # vi: set ft=ruby :

Vagrant.require_version ">= 1.6.0"

# Defaults for config options defined in CONFIG
$num_instances = 1
$update_channel = "alpha"
$vb_gui = false
$vb_memory = 1024
$vb_cpus = 1
$ip = "192.168.33.66"


Vagrant.configure("2") do |config|
  config.vm.box = "coreos-%s" % $update_channel
  config.vm.box_version = ">= 308.0.1"
  config.vm.box_url = "http://%s.release.core-os.net/amd64-usr/current/coreos_production_vagrant.json" % $update_channel

  config.vm.network "forwarded_port", guest: 8500, host: 8500, auto_correct: true

  config.vm.provider :virtualbox do |v|
    # On VirtualBox, we don't have guest additions or a functional vboxsf
    # in CoreOS, so tell Vagrant that so it can be smarter.
    v.check_guest_additions = false
    v.functional_vboxsf     = false
  end

  # plugin conflict
  if Vagrant.has_plugin?("vagrant-vbguest") then
    config.vbguest.auto_update = false
  end

  
  config.vm.hostname = "core-os"

  if $expose_docker_tcp
    config.vm.network "forwarded_port", guest: 2375, host: ($expose_docker_tcp + i - 1), auto_correct: true
  end

  config.vm.provider :virtualbox do |vb|
    vb.gui = $vb_gui
    vb.memory = $vb_memory
    vb.cpus = $vb_cpus
  end

  
  config.vm.network :private_network, ip: $ip

  # Uncomment below to enable NFS for sharing the host machine into the coreos-vagrant VM.
  config.vm.synced_folder ".", "/home/core/share", id: "core", :nfs => false, :mount_options => ['nolock,vers=3,noatime']
  
end
