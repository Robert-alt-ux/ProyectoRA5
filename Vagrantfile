Vagrant.configure("2") do |config|

  # Box Ubuntu
  config.vm.box = "ubuntu/jammy64"

  # Nombre de la VM
  config.vm.hostname = "ubuntu-vm"

  # Forwarding WordPress
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # Carpeta sincronizada
  config.vm.synced_folder ".", "/vagrant"

  # Recursos VM
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 2
  end

  # Provisioning
  config.vm.provision "shell", inline: <<-SHELL

    # Actualizar sistema
    sudo apt update

    # Instalar dependencias
    sudo apt install -y \
      ca-certificates \
      curl \
      gnupg \
      lsb-release \
      git

    # Instalar Docker
    sudo mkdir -p /etc/apt/keyrings

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
      sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

    echo \
      "deb [arch=$(dpkg --print-architecture) \
      signed-by=/etc/apt/keyrings/docker.gpg] \
      https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    sudo apt update

    sudo apt install -y \
      docker-ce \
      docker-ce-cli \
      containerd.io \
      docker-buildx-plugin \
      docker-compose-plugin

    # Permisos docker
    sudo usermod -aG docker vagrant

    # Entrar en carpeta proyecto
    cd /vagrant

    # Levantar contenedores
    sudo docker compose down
    sudo docker compose up -d --force-recreate

  SHELL

end