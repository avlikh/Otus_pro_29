Vagrant.configure("2") do |config|
    # Настройка PXE-сервера
    config.vm.define "pxeserver" do |server|
      server.vm.box = 'bento/ubuntu-22.04'
      server.vm.hostname = 'pxeserver'  # Исправлено host_name → hostname
  
      # Проброс порта
      server.vm.network "forwarded_port", guest: 80, host: 8080
  
      # Настройка сетевых интерфейсов
      server.vm.network :private_network, ip: "10.0.0.20", virtualbox__intnet: 'pxenet'
      server.vm.network :private_network, ip: "192.168.50.10", adapter: 3
  
      # Настройка параметров виртуальной машины
      server.vm.provider "virtualbox" do |vb|
        vb.memory = "4096"
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      end
  
      # Запуск Ansible
      server.vm.provision "ansible" do |ansible|
        ansible.playbook = "provision.yml"  # Путь к Playbook
        ansible.config_file = "ansible.cfg" # Файл конфигурации Ansible
        ansible.become = true                       # Использование sudo
        ansible.limit = "all"                       # Применение ко всем узлам
        ansible.extra_vars = { "ansible_tags" => "pxe_manual" } # Добавляем тег
      end
    end
  
    # Настройка PXE-клиента
    config.vm.define "pxeclient" do |pxeclient|
      pxeclient.vm.box = 'bento/ubuntu-22.04'
      # pxeclient.vm.box = 'seskion/ubuntu-20.04-efi'  # Альтернативный образ
      pxeclient.vm.hostname = 'pxeclient'  # Исправлено host_name → hostname
  
      # Настройка сети
      pxeclient.vm.network :private_network, ip: "192.168.50.11"
  
      # Настройка параметров виртуальной машины
      pxeclient.vm.provider :virtualbox do |vb|
        vb.memory = "4096"
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize [
          'modifyvm', :id,
          '--nic1', 'intnet',
          '--intnet1', 'pxenet',
          '--nic2', 'nat',
          '--boot1', 'net',
          '--boot2', 'none',
          '--boot3', 'none',
          '--boot4', 'none'
        ]
      end
    end
  end
  