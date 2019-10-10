IMAGE_NAME = "generic/ubuntu1804"
N = 2
Vagrant.configure("2") do |config|
	config.ssh.insert_key = false

	config.vm.provider :libvirt do |v|
		v.uri = 'qemu+unix:///system'
		v.driver = 'kvm'
		v.memory = '2048'
		v.cpus = 2
	end

	config.vm.define "master" do |master|
		master.vm.box = IMAGE_NAME
		master.vm.network "private_network", ip: "192.168.39.10"
		master.vm.hostname = "master"
	end

	(1..N).each do |i|
		config.vm.define "worker-#{i}" do |worker|
			worker.vm.box = IMAGE_NAME
			worker.vm.hostname = "worker-#{i}"
			worker.vm.network "private_network", ip: "192.168.39.#{i+10}"
		end
	end

    config.vm.provision "ansible" do |ansible|
        ansible.groups = {
            "masters" => ["master"],
            "workers" =>  ["worker-[1:#{N}]"]
        }
        ansible.playbook = "provisioning/kube.yml"
    end

end
