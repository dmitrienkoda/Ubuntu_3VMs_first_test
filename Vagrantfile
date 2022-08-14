# -*- mode: ruby -*-
# vi: set ft=ruby :

# Префикс для Internal сети
#INTERNAL_NET="192.168.15."
# Домен который будем использовать для всей площадки
DOMAIN="local.test"

# Массив из хешей, в котором заданы настройки для каждой виртуальной машины
servers=[
  {
    :hostname => "VM1." + DOMAIN,
    :ip_int => "192.168.56.10",
    :ram => 4000,
    :cpu => 3,
    :sshfwport => "2201",
    :fwport => "8080"
  },
  {
    :hostname => "VM2." + DOMAIN,
    :ip_int => "192.168.56.20",
    :ram => 3000,
    :cpu => 2,
    :sshfwport => "2202",
    :fwport => "8081"
  },
  {
    :hostname => "VM3." + DOMAIN,
    :ip_int => "192.168.56.30",
    :ram => 2000,
    :cpu => 2,
    :sshfwport => "2203",
    :fwport => "8082"
#   :hdd_name => "db1_hdd.vdi",
#   :hdd_size => "10000"
  },
  ]
 
# Начало конфигурации Vagrant версии 2
Vagrant.configure("2") do |config|
    # Проходим по элементах массива "servers"
    servers.each do |machine|
      # Применяем конфигурации для каждой машины. Имя машины(как ее будет видно в Vbox GUI) находится в переменной "machine[:hostname]"
      config.vm.define machine[:hostname] do |node|
        # Поднять машину из образа "Ubuntu22.04"
        node.vm.box = "Ubuntu22.04"
        # Пул портов, который будет использоваться для подключения к каждый VM через 127.0.0.1
        node.vm.usable_port_range = (2200..2250)
        # Hostname который будет присвоен VM (самой ОС)
        node.vm.hostname = machine[:hostname]
        #node.vm.network "private_network", ip: machine[:ip_int], virtualbox__intnet: "Intel(R) Ethernet Connection (6) I219-LM"
        node.vm.network "private_network", ip: machine[:ip_int]
        node.vm.network "forwarded_port", guest: 22, host: machine[:sshfwport], id: "ssh"
        node.vm.network "forwarded_port", guest: 80, host: machine[:fwport]
          # Тонкие настройки для VBoxManage
          node.vm.provider "virtualbox" do |vb|
          # Размер RAM памяти
          vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
          # Количество ядер
          vb.customize ["modifyvm", :id, "--cpus", machine[:cpu]]
          end
      end
    end
end
