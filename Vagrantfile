Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

  config.vm.define "nginx" do |cfg|
    cfg.vm.box = "generic/ubuntu2004"
    cfg.vm.network "private_network", ip: "192.168.50.20"
    cfg.vm.network "forwarded_port", guest: 22, host: 60510, auto_correct: true, id: "ssh"
    cfg.vm.hostname = "nginx"
    
    cfg.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.cpus = 1
      v.name = "nginx"
      v.linked_clone = true
    end

    # ssh 비밀번호인증 활성화
    cfg.vm.provision "shell", inline: <<-SCRIPT
      sed -i -e 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
      systemctl restart sshd
    SCRIPT

    cfg.vm.provision "shell", inline: <<-SCRIPT
      apt-get update
      apt-get install nginx tmux vim git -y
    SCRIPT
  end
end