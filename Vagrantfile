# -*- mode: ruby -*-
# vi: set ft=ruby :
options = {
  :cores => 2,
  :memory => 3000,
}
Vagrant.configure("2") do |config|
    config.vm.network "public_network"
    config.vm.box = "centos"
    config.vm.hostname = "micro-qa"
    config.vm.synced_folder ".", "/vagrant", owner: "root", group: "root"
    config.vm.provider :aws do |aws, override|
        override.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
        override.ssh.pty = true
        override.ssh.private_key_path = "/Users/alicehubenko/keypair/id_rsa"
        aws.access_key_id ="1YCMC3ZSCW8UF2GUE1B3U" 
        aws.secret_access_key = "4fV20keM4AQiRMBejxxgYhYxQSFnd7vamQ8IsdCh" 
        aws.instance_type = "m1.xlarge"
        aws.ami = "emi-80F94DD2"
        aws.security_groups = ["default"]
        aws.region = "eucalyptus"
        aws.instance_ready_timeout = 600
        aws.endpoint = "http://10.111.1.136:8773/services/Eucalyptus"
        aws.keypair_name = "mykey"
        override.ssh.username ="root"
        aws.block_device_mapping = [
        {
            :DeviceName => "/dev/sda", 
            "Ebs.VolumeSize" => 10
        }]
        aws.tags = {
                Name: "Micro QA",
        }
    end
    config.vm.network :forwarded_port, guest: 80, host: 8080
    config.vm.provider :vmware_fusion do |v, override|
        if config.vm.box == "centos"
          override.vm.box_url = "https://dl.dropbox.com/u/5721940/vagrant-boxes/vagrant-centos-6.4-x86_64-vmware_fusion.box"
        else
          override.vm.box_url = "http://grahamc.com/vagrant/ubuntu-12.04.2-server-amd64-vmware-fusion.box"
        end
        v.vmx["memsize"] = options[:memory].to_i
        v.vmx["numvcpus"] = options[:cores].to_i
        v.vmx["vhv.enable"] = "true"
    end
    config.vm.provider :virtualbox do |v, override|
        if config.vm.box == "centos"
          override.vm.box_url = "http://developer.nrel.gov/downloads/vagrant-boxes/CentOS-6.4-x86_64-v20130427.box"
        else
          override.vm.box_url = "http://grahamc.com/vagrant/ubuntu-12.04-omnibus-chef.box"
        end
        v.customize [ "modifyvm", :id, "--memory", options[:memory].to_i, "--cpus", options[:cores].to_i]
    end
    config.vm.provision :shell, :path => "deploy_#{config.vm.box}.sh"
end
