# -*- mode: ruby -*-
# vi: set ft=ruby :
class Hash
  def slice(*keep_keys)
    h = {}
    keep_keys.each { |key| h[key] = fetch(key) if has_key?(key) }
    h
  end unless Hash.method_defined?(:slice)
  def except(*less_keys)
    slice(*keys - less_keys)
  end unless Hash.method_defined?(:except)
end

require 'vagrant-aws'
Vagrant.configure('2') do |config|
    config.vm.box = 'dummy'
    config.vm.synced_folder '.', '/vagrant', disabled: true
    config.vm.define 'apache'
    config.vm.hostname = 'apache'
    config.vm.provider 'aws' do |aws, override|	
	    aws.access_key_id = ENV['AWS_ACCESS_KEY']
    	aws.secret_access_key = ENV['AWS_SECRET_KEY']
	    aws.keypair_name = 'EC2-Vagrant'
	    aws.instance_type = 't2.micro'
    	aws.region = 'us-east-1'
      aws.availability_zone = 'us-east-1e'
    	aws.ami = "ami-0cff7528ff583bf9a"
    	aws.security_groups = 'sg-080015f68aed4dcbf'
    	aws.subnet_id = 'subnet-08a6a35b49b898ce6'
      aws.tags = {
        'Name' => 'EC2-Provisioned by Vagrant'
        }
    	override.ssh.username = 'ec2-user'
    	override.ssh.private_key_path = ENV['AWS_PRIVATE_KEY']
      
      config.vm.provision "ansible" do |ansible|
        ansible.verbose = "v"
        ansible.playbook = "ansible-playbook-apache.yml"
      end
  end
end
