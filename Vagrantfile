# frozen_string_literal: true

NUM_MASTER_NODE = 1
NUM_WORKER_NODE = 2

IP_NW = '192.168.56.'
MASTER_IP_START = 1
NODE_IP_START = 2

Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/bionic64'

  config.vm.box_check_update = false

  # Provision Master Nodes
  (1..NUM_MASTER_NODE).each do |i|
    config.vm.define 'kubemaster' do |node|
      # Name shown in the GUI
      node.vm.provider 'virtualbox' do |vb|
        vb.name = 'kubemaster'
        vb.memory = 3000
        vb.cpus = 3
      end
      node.vm.hostname = 'kubemaster'
      node.vm.network :private_network, ip: IP_NW + (MASTER_IP_START + i).to_s
      node.vm.network 'forwarded_port', guest: 22, host: (2710 + i).to_s

      node.vm.provision 'setup-hosts', type: 'shell', path: 'scripts/setup-hosts.sh' do |s|
        s.args = ['enp0s8']
      end

      node.vm.provision 'setup-dns', type: 'shell', path: 'scripts/update-dns.sh'
      node.vm.provision 'setup-dns', type: 'shell', path: 'scripts/install_kubeadm.sh'
    end
  end

  # Provision Worker Nodes
  (1..NUM_WORKER_NODE).each do |i|
    config.vm.define "kubenode0#{i}" do |node|
      node.vm.provider 'virtualbox' do |vb|
        vb.name = "kubenode0#{i}"
        vb.memory = 2048
        vb.cpus = 2
      end
      node.vm.hostname = "kubenode0#{i}"
      node.vm.network :private_network, ip: IP_NW + (NODE_IP_START + i).to_s
      node.vm.network 'forwarded_port', guest: 22, host: (2720 + i).to_s

      node.vm.provision 'setup-hosts', type: 'shell', path: 'scripts/setup-hosts.sh' do |s|
        s.args = ['enp0s8']
      end

      node.vm.provision 'setup-dns', type: 'shell', path: 'scripts/update-dns.sh'
      node.vm.provision 'setup-dns', type: 'shell', path: 'scripts/install_kubeadm.sh'
    end
  end
end
