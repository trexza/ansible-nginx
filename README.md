#Install a Blog Site in 10 mins

This article will demonstrate how to deploy Wordpress blog on a virtual machine instance in about 10 mins.

### Pre-requisites

- Familiarity on simple tools - Vagrant, Ansible, VirtualBox, and WordPress
- Vagrant and VirtualBox installed on host machine
- Intent to get started with the habit of blogging (for benefit of others ;-) )

### What It Does

- Expects CentOS/RHEL 6.x VM
- Uses Ansible 1.2 or newer
- Installs Nginx, MySQL
- Copies and Configures Wordpress

This will deploy a VM with simple all-in-one configuration of the popular
WordPress blogging platform and CMS, frontend by the Nginx web server and the
PHP-FPM process manager.

### In few simple steps

- Clone the git repo at *https://github.com/ansible/ansible-examples.git* (Note: It has several examples for ansible, whereas we require only the wordpress-nginx example for this demo). You could also download from my github which includes the Vagrant file and contains only the code for wordpress-nginx.
- Install Vagrant and VirtualBox on host where you will launch the VM
- Create a Vagrant file for launching the VM
- Configure Vagrant to map the Wordpress (wordpress-nginx) host folder in your host file system to a mount on the VM.  
```
VAGRANT_API_VERSION = "2"
Vagrant.configure(VAGRANT_API_VERSION) do |config|
  config.vm.box = "CentOS 6.4 x86_64"
  #config.vm.box_url = "https://github.com/2creatives/vagrant-centos/releases/download/v6.4.2/centos64-x86_64-20140116.box"
  if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
  config.vm.synced_folder ".", "/<mount>", mount_options: ["dmode=700,fmode=600"]
  ...
```
- Add the setup script for installing Ansible to the VM script at launch
```
bootstrap_ansible.sh
```
- Include the roles to Ansible script for completing the Wordpress installation after successful VM initialisation. 
```
d.vm.provision :shell, path: "<mount>/scripts/bootstrap_ansible.sh"
d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook -i <mount>/<path>/hosts/customhosts.txt /<mount>/<path>/site.yml -c local 
```
- Launch the VM  (Note: Launch time could vary depending on whether you have already added the Vbox to local cache.. and quality of your internet connection of course)
```
vagrant up <name of the VM>
```
- If no errors, the blog site will be accessible on the URL ```http://<IP of the VM>:80```

###What Next
This deployment will be best suited for testing i.e. it doesn't include other configurations for a real blog if you were to consider a cloud deployment.
Watch out for next one in this series which will cover cloud deployment of the blog site.