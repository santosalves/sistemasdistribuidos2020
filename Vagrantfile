$essencial = <<-SCRIPT
sudo yum install epel-release -y
sudo yum install htop git vim mlocate -y
sudo service firewalld stop
sudo systemctl disable firewalld
sudo setenforce 0
SCRIPT

$docker = <<-SCRIPT
sudo curl -fsSL https://get.docker.com | bash
sudo usermod -aG docker vagrant
sudo systemctl enable docker
sudo systemctl start docker
SCRIPT

$nginx = <<-SCRIPT
sudo yum install nginx keepalived -y
sudo systemctl enable nginx
sudo systemctl enable keepalived
sudo cp /vagrant/nginx.conf /etc/nginx/nginx.conf
sudo mkdir /etc/nginx/conf2.d
sudo cp /vagrant/docker-cluster.conf /etc/nginx/conf2.d/docker-cluster.conf
SCRIPT

$nginx_keepalived_master = <<-SCRIPT
sudo cp /vagrant/keepalived-master.conf /etc/keepalived/keepalived.conf
sudo cp /vagrant/index-master.html /usr/share/nginx/html/index.html
sudo systemctl start nginx
sudo systemctl start keepalived
SCRIPT

$nginx_keepalived_backup = <<-SCRIPT
sudo cp /vagrant/keepalived-backup.conf /etc/keepalived/keepalived.conf
sudo cp /vagrant/index-backup.html /usr/share/nginx/html/index.html
sudo systemctl start nginx
sudo systemctl start keepalived
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.define "sd_docker01", primary: true do |sd_docker01|
    # Conf básica na VM
    sd_docker01.vm.provider "virtualbox" do |v|
      v.name = "sd_docker01"
      v.memory = 1024
      v.cpus = 2
    end  
    # Conf básica no S.O
    sd_docker01.vm.box = "centos/7"
    sd_docker01.vm.hostname = "docker01"
    sd_docker01.vm.network "public_network", ip: "172.35.20.90"
    # Preparando a máquina para rodar o cluster swarm e instalando utilitários
    sd_docker01.vm.provision "shell",
      inline: $essencial
    sd_docker01.vm.provision "shell",
      inline: $docker
    sd_docker01.vm.provision "shell",
      inline: "docker swarm init --advertise-addr eth1"
  end  

  config.vm.define "sd_docker02" do |sd_docker02|
    # Conf básica na VM
    sd_docker02.vm.provider "virtualbox" do |v|
      v.name = "sd_docker02"
      v.memory = 1024
      v.cpus = 2
    end  
    # Conf básica no S.O
    sd_docker02.vm.box = "centos/7"
    sd_docker02.vm.hostname = "docker02"
    sd_docker02.vm.network "public_network", ip: "172.35.20.91"
    # Preparando a máquina para rodar o cluster swarm e instalando utilitários
    sd_docker02.vm.provision "shell",
      inline: $essencial
    sd_docker02.vm.provision "shell",
      inline: $docker
  end
  
  config.vm.define "sd_docker03" do |sd_docker03|
    # Conf básica na VM
    sd_docker03.vm.provider "virtualbox" do |v|
      v.name = "sd_docker03"
      v.memory = 1024
      v.cpus = 2
    end  
    # Conf básica no S.O
    sd_docker03.vm.box = "centos/7"
    sd_docker03.vm.hostname = "docker03"
    sd_docker03.vm.network "public_network", ip: "172.35.20.92"
    # Preparando a máquina para rodar o cluster swarm e instalando utilitários
    sd_docker03.vm.provision "shell",
      inline: $essencial
    sd_docker03.vm.provision "shell",
      inline: $docker
  end

  config.vm.define "sd_docker04" do |sd_docker04|
    # Conf básica na VM
    sd_docker04.vm.provider "virtualbox" do |v|
      v.name = "sd_docker04"
      v.memory = 1024
      v.cpus = 2
    end  
    # Conf básica no S.O
    sd_docker04.vm.box = "centos/7"
    sd_docker04.vm.hostname = "docker04"
    sd_docker04.vm.network "public_network", ip: "172.35.20.93"
    # Preparando a máquina para rodar o cluster swarm e instalando utilitários
    sd_docker04.vm.provision "shell",
      inline: $essencial
    sd_docker04.vm.provision "shell",
      inline: $docker
  end 

  config.vm.define "sd_nginx01" do |sd_nginx01|
    # Conf básica na VM
    sd_nginx01.vm.provider "virtualbox" do |v|
      v.name = "sd_nginx01"
      v.memory = 512
      v.cpus = 1
    end  
    # Conf básica no S.O
    sd_nginx01.vm.box = "centos/7"
    sd_nginx01.vm.hostname = "nginx01"
    sd_nginx01.vm.network "public_network", ip: "172.35.20.94"
    # Preparando a máquina para rodar o cluster swarm e instalando utilitários
    sd_nginx01.vm.provision "shell",
      inline: $essencial
    sd_nginx01.vm.provision "shell",
      inline: $nginx
    sd_nginx01.vm.provision "shell",
      inline: $nginx_keepalived_master
  end

  config.vm.define "sd_nginx02" do |sd_nginx02|
    # Conf básica na VM
    sd_nginx02.vm.provider "virtualbox" do |v|
      v.name = "sd_nginx02"
      v.memory = 512
      v.cpus = 1
    end  
    # Conf básica no S.O
    sd_nginx02.vm.box = "centos/7"
    sd_nginx02.vm.hostname = "nginx02"
    sd_nginx02.vm.network "public_network", ip: "172.35.20.95"
    # Preparando a máquina para rodar o cluster swarm e instalando utilitários
    sd_nginx02.vm.provision "shell",
      inline: $essencial
    sd_nginx02.vm.provision "shell",
      inline: $nginx
    sd_nginx02.vm.provision "shell",
      inline: $nginx_keepalived_backup
  end

end

