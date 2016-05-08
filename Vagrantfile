Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.box = "fedora-23"

  N = 3
  (1..N).each do |id|
   config.vm.define "server#{id}" do |server|
      server.vm.network "private_network", ip: "192.168.100.10#{id}"
   end
  end

  config.vm.define "client" do |client|
    client.vm.network "private_network", ip: "192.168.100.104" 
    client.vm.provision :ansible do |ansible|
	ansible.groups = {
                "servers" => ["server1","server2","server3"],
                "client" => ["client"],
                "all_groups:children" => ["servers", "client"]
        }
        ansible.playbook = "demo.yml"
        ansible.limit = "all"
	ansible.raw_ssh_args = ["-o ServerAliveInterval=20"]
        #ansible.verbose = "v"
    end
  end
end
