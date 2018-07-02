# -- mode: ruby --
# vi: set ft=ruby :

#
#Im ersten Abschnitt wird das Guest-OS der VM bestimmt.
Vagrant.configure(2) do |config|
  #config.ssh.username = "admin"
  #config.ssh.password = "admin"
    
 end

#Virtualbox für FTP
#Im ersten Abschnitt wird das Guest-OS der VM bestimmt.
  config.vm.define "ftp" do |ftp|
    ftp.vm.box = "ubuntu/xenial64"
    ftp.vm.hostname = "ftp"
    ftp.vm.network "private_network", ip: "192.168.6.6"
    ftp.vm.network "forwarded_port", guest:3306, host:3306, auto_correct: true
    ftp.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"  
    end
    ftp.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        #FTP Server installieren
        sudo apt-get -y install pure-ftpd-common pure-ftpd
        #FTP Server konfigurieren
        sudo groupadd ftpgroup
        sudo useradd -g ftpgroup -d /dev/null -s /etc ftpuser
        sudo pure-pw useradd samjoy -u ftpuser -g ftpgroup -d /home/pubftp/samjoy -N 10
        #FTP Server neustarten
	#sudo service pure-ftpd-common pure-ftpd restart
        sudo /home/pubftp/samjoy restart
        #Tastaturlayout anpassen
        sudo sed -i 's/XKBLAYOUT="us"/XKBLAYOUT="ch"/g' /etc/default/locale
        #Local Firewall installieren
        sudo apt-get -y install ufw gufw 
        sudo ufw allow from 10.0.2.2 to any port 22
        sudo ufw --force enable
SHELL
 end
