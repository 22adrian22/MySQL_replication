# Vagrantfile

Vagrant.configure("2") do |config|
    # Definir la caja base
    config.vm.box = "ubuntu/jammy64"
  
    # Configuración del servidor maestro
    config.vm.define "mysql-master" do |master|
      master.vm.hostname = "mysql-master"
      master.vm.network "private_network", ip: "192.168.57.101"
    end
  
    # Configuración del servidor esclavo
    config.vm.define "mysql-slave" do |slave|
      slave.vm.hostname = "mysql-slave"
      slave.vm.network "private_network", ip: "192.168.57.102"
    end
end
  