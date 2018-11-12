HOSTNAME="server"
EXT_NET="192.168.49.1"
MAC_EXT="0800278C22"
INT_NET="192.168.0.1"
INT_MASK="255.255.255.0"
MAC_INT="0800276108"
BOXNAME="ubuntu/xenial64"
RAM="512"
NAME_GENERATE=""
NUM_OF_MACHINES=""

#6.times.map { '%02x' % rand(0..255) }.join('').upcase
#def mac_generate(mac)
#  return "mac_ext" + mac
#end

virtual_servers=[
  {
    :boxname => BOXNAME,
    :hostname => HOSTNAME + "1",
    :int_addr => INT_NET + "1", 
    :ext_addr => EXT_NET + "10",
    :ram => RAM,
    :mac_ext => MAC_EXT + "CE",
    :mac_int => MAC_INT + "DE"
    },
    {
      :boxname => BOXNAME,
      :hostname => HOSTNAME + "2",
      :int_addr => INT_NET + "2", 
      :ext_addr => EXT_NET + "20",
      :ram => RAM,
      :mac_ext => MAC_EXT + "AB",
      :mac_int => MAC_INT + "DC"
    }
  ]

  Vagrant.configure("2") do |config|
    virtual_servers.each do |machine|
      config.vm.define machine [:hostname] do |node|
        node.vm.box = machine [:boxname]
        node.vm.hostname = machine [:hostname]
        if(node.vm.hostname == "server1")
          node.vm.network "public_network", ip: machine[:ext_addr], mac: machine[:mac_ext]
          node.vm.network "private_network", ip: machine[:int_addr], netmask: INT_MASK, mac: machine [:mac_int]
          #node.vm.provision "shell",
          #run: "always",
          #path: "./ansible.sh"
        else
          node.vm.network "public_network", ip: machine[:ext_addr], mac: machine[:mac_ext]
          node.vm.network "private_network", ip: machine[:int_addr], netmask: INT_MASK, mac: machine [:mac_int]
        end
          node.vm.provider "virtualbox" do |vb|
          vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
          vb.name = machine[:hostname]
        end
      end
    end 
  end