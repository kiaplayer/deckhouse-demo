Vagrant.configure('2') do |config|

    cluster = {
        'deckhouse' => { 'ip' => '192.168.56.10', 'cpus' => 4, 'mem' => 8096 },
        'worker'    => { 'ip' => '192.168.56.11', 'cpus' => 2, 'mem' => 2048 },
    }

    cluster.each_with_index do |(hostname, info), index|
        config.vm.define hostname do |box_config|
            box_config.vm.box = 'ubuntu/focal64'
            box_config.vm.hostname = hostname
            box_config.vm.provider 'virtualbox' do |v|
              v.memory = info['mem']
              v.cpus = info['cpus']
            end
            box_config.vm.network :private_network, ip: info['ip']
        end
    end

end
