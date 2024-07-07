
Vagrant.configure("2") do |config|
 
 
  config.vm.box = "base"

  servers = [
    {
      :hostname => "centos.example.com",
      :box => "bento/centos-7",
      :ip => "10.10.30.30",
      :ssh_port => '2210',
      :memory => '4096',
      :cpu => '2'
    },
	{
      :hostname => "centos1.example.com",
      :box => "bento/centos-7",
      :ip => "10.10.30.31",
      :ssh_port => '2215',
      :memory => '2048',
      :cpu => '2'
	},
	{
      :hostname => "centos2.example.com",
      :box => "bento/centos-7",
      :ip => "10.10.30.32",
      :ssh_port => '2222',
      :memory => '1024',
      :cpu => '1'
	},
	{
      :hostname => "ubuntu.example.com",
      :box => "bento/ubuntu-22.04",
      :ip => "10.10.30.33",
      :ssh_port => '2323',
      :memory => '8192',
      :cpu => '4'
	}
  ]

  servers.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box = machine[:box]
      node.vm.hostname = machine[:hostname]

      node.vm.network :private_network, ip: machine[:ip]
      node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"

      node.vm.provider :virtualbox do |v|
        v.customize ["modifyvm", :id, "--name", machine[:hostname]]
        v.customize ["modifyvm", :id, "--memory", machine[:memory]]
        v.customize ["modifyvm", :id, "--cpus", machine[:cpu]]
      end
    end
  end

  id_rsa_key_pub = File.read(File.join(Dir.home, ".ssh", "id_rsa.pub"))

  config.vm.provision :shell,
    inline: "echo 'appending SSH public key to ~vagrant/.ssh/authorized_keys' && echo '#{id_rsa_key_pub}' >> /home/vagrant/.ssh/authorized_keys && chmod 600 /home/vagrant/.ssh/authorized_keys"

  config.ssh.insert_key = false
end
