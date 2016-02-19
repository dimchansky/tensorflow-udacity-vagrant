# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "default", autostart: false do |default|
    default.vm.provision "docker"

    # The following line terminates all ssh connections. Therefore
    # Vagrant will be forced to reconnect.
    # That's a workaround to have the docker command in the PATH
    default.vm.provision "shell", inline:
       "ps aux | grep 'sshd:' | awk '{print $2}' | xargs kill"

    default.vm.box = "phusion/ubuntu-14.04-amd64"

    default.vm.network "forwarded_port", guest: 8888, host: 8888
    default.vm.network "forwarded_port", guest: 6006, host: 6006

    default.vm.synced_folder ".", "/vagrant", disabled: true
    default.vm.synced_folder "./assignments", "/assignments", owner: "root", group: "root", :mount_options => ["dmode=777", "fmode=666"]

    default.vm.provider :virtualbox do |vb|
      vb.memory = 8192
      vb.cpus = 2
    end
  end

  config.vm.define "tensorflow-udacity" do |a|
    a.vm.provider "docker" do |d|
      d.image = "b.gcr.io/tensorflow-udacity/assignments"
      d.name = "tensorflow-udacity"
      d.ports = ["8888:8888", "6006:6006"]
      d.volumes = ["/assignments:/assignments"]
      d.cmd = ["/run_jupyter.sh", "--notebook-dir=/assignments"]
      d.remains_running = false
      d.vagrant_vagrantfile = __FILE__
    end
  end
  
end