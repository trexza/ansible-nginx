# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANT_API_VERSION = "2"
Vagrant.configure(VAGRANT_API_VERSION) do |config|
  config.vm.box = "centos64-x86_64"
  if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
  config.vm.synced_folder ".", "/ansible", mount_options: ["dmode=700,fmode=600"]
  else
  config.vm.synced_folder ".", "/ansible"
  end

  
#  config.vm.box = "ansible-ubuntu-1204-i386"
#  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-i386-vagrant-disk1.box"
  
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
  end
  
  config.vm.define "contrl" do |d|
    d.vm.hostname = "contrl"
    d.vm.network "private_network", ip: "192.168.61.100"
    d.vm.provision :shell, path: "scripts/bootstrap_ansible.sh"
    d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook -i /vagrant/hosts/customhosts.txt /vagrant/control.yml -c local"
    d.vm.provider "virtualbox" do |v|
      v.memory = 2048
    end
  end 
  
  config.vm.define "awscontrol" do |d|
    d.vm.hostname = "awscontrol"
    d.vm.network "private_network", ip: "192.168.61.200"
    d.vm.provision :shell, path: "scripts/bootstrap_ansible.sh"
    d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook -i /vagrant/hosts/customhosts.txt /vagrant/awscontrol.yml -c local"
    d.vm.provider "virtualbox" do |v|
      v.memory = 2048
    end
  end 
  
  config.vm.define "db" do |db| 
  	db.vm.network :private_network, ip: "192.168.61.11" 
  end
  
  config.vm.define "www" do |www| 
  www.vm.network :private_network, ip: "192.168.61.12" 
  end 
  
  config.vm.define "app" do |app|
     app.vm.hostname = "app1"
     app.vm.network :private_network, ip: "192.168.61.14"
     if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
     app.vm.synced_folder "../cloud-data", "/data1", mount_options: ["dmode=700,fmode=600"]
     else
     app.vm.synced_folder "../cloud-data", "/data1"
     end
  end  
  
    config.vm.define "app1" do |app1|
       app1.vm.hostname = "app2"
       app1.vm.network :private_network, ip: "192.168.61.15"
       if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
       app1.vm.synced_folder "../cloud-data", "/data1", mount_options: ["dmode=700,fmode=600"]
       else
       app1.vm.synced_folder "../cloud-data", "/data1"
       end
  end  
  
  config.vm.define "lb" do |lb| 
  lb.vm.network :private_network, ip: "192.168.61.13" 
  end 
  
  
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
end