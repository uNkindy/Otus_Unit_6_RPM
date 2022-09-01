# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'open3'
require 'fileutils'

Vagrant.configure("2") do |config|

config.vm.define "server" do |server|
  config.vm.box = 'almalinux/8'

  server.vm.host_name = 'server'
  server.vm.network :private_network, ip: "192.168.56.240"

  server.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  end


end
