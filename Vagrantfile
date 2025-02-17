NODE_ROLES = ["server-0", "server-1", "server-2"]
NODE_BOXES = ['generic/ubuntu2204', 'generic/ubuntu2204', 'generic/ubuntu2204']
# Virtualbox >= 6.1.28 require `/etc/vbox/network.conf` for expanded private networks 

def provision(vm, role, node_num)
  vm.box = NODE_BOXES[node_num]
  vm.hostname = role
  # We use a private network because the default IPs are dynamically assigned 
  # during provisioning. This makes it impossible to know the server-0 IP when 
  # provisioning subsequent servers and agents. A private network allows us to
  # # assign static IPs to each node, and thus provide a known IP for the API endpoint.
  # node_ip = "#{NETWORK_PREFIX}.#{100+node_num}"
  # # An expanded netmask is required to allow VM<-->VM communication, virtualbox defaults to /32
  # vm.network "private_network", ip: node_ip, netmask: "255.255.255.0"

  # vm.provision "ansible", run: 'once' do |ansible|
  #   ansible.compatibility_mode = "2.0"
  #   ansible.playbook = "playbooks/site.yml"
  #   ansible.groups = {
  #     "server" => NODE_ROLES.grep(/^server/),
  #     "agent" => NODE_ROLES.grep(/^agent/),
  #     "k3s_cluster:children" => ["server", "agent"],
  #   }
  #   ansible.extra_vars = {
  #     k3s_version: "v1.28.14+k3s1",
  #     api_endpoint: "#{NETWORK_PREFIX}.100",
  #     # Required for vagrant ansible provisioner
  #     token: "myvagrant",
  #     # Required to use the private network configured above
  #     extra_server_args: "--node-external-ip #{node_ip} --flannel-iface eth1", 
  #     extra_agent_args: "--node-external-ip #{node_ip} --flannel-iface eth1",
  #     # Airgap setup, left as reference
  #     # airgap_dir: "./my_airgap",
  #     # Optional, left as reference for ruby-ansible syntax
  #     # extra_service_envs: [ "NO_PROXY='localhost'" ],
  #     # server_config_yaml: <<~YAML
  #     #   write-kubeconfig-mode: 644
  #     #   kube-apiserver-arg:
  #     #     - advertise-port=1234
  #     # YAML
  # #   }
  # end
end

Vagrant.configure("2") do |config|
  # Default provider is libvirt, virtualbox is only provided as a backup
  config.vm.provider "qemu" do |qe|
    qe.qemu_dir = "/usr/bin/"
    qe.arch = "x86_64"
    qe.memory = "2048"
    qe.smp = "4"
    qe.machine = "q35"
    qe.cpu = "max"
    qe.net_device = "virtio-net-pci"
  end
  
  NODE_ROLES.each_with_index do |name, i|
    config.vm.define name do |node|
      provision(node.vm, name, i)
    end
  end

end