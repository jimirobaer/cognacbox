# -*- mode: ruby -*-
# vi: set ft=ruby :

# Installation of plugins
required_plugins = %w(vagrant-hostmanager)
plugins_to_install = required_plugins.select { |plugin| not Vagrant.has_plugin? plugin }
if not plugins_to_install.empty?
	puts "Installing plugins: #{plugins_to_install.join(' ')}"
		if system "vagrant plugin install #{plugins_to_install.join(' ')}"
    	exec "vagrant #{ARGV.join(' ')}"
  else
    	abort "Installation of one or more plugins has failed. Aborting."
	end
end

Vagrant.configure("2") do |config|

    config.vm.box = "reddingwebpro/cognacbox"
    config.vm.hostname = "client.local"
    config.vm.box_version = "2.3"
    
    config.vm.network "forwarded_port", guest: 80, host: 8080
    config.vm.network "private_network", ip: "192.168.33.10"

    # Syncing NFS
    config.vm.synced_folder "~/Web", "/var/www/public", :nfs => true, :mount_options => ['noatime,soft,nolock,vers=3,udp,proto=udp,udp,rsize=8192,wsize=8192,namlen=255,timeo=10,retrans=3,nfsvers=3,actimeo=1']

    # Hostmanager
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.vm.provision :hostmanager

    config.vm.provider "virtualbox" do |v|
        v.memory = 4096
        v.cpus = 4
    end

end
