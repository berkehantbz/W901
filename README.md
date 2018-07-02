# W901
Linux/UNIX
FTP-Server
Die FTP VM hat folgende Spezifikationen (Die Änderungen kann man im Vagrant-File vornehmen):

IP: 192.168.6.6
Hostname: ftp
RAM: 1024 MB
VM Box: Ubuntu
config.vm.define "ftp" do |ftp|
  ftp.vm.box = "ubuntu/xenial64"
  ftp.vm.hostname = "ftp"
  ftp.vm.network "private_network", ip: "192.168.6.6"
  ftp.vm.network "forwarded_port", guest:3306, host:3306, auto_correct: true
  ftp.vm.provider "virtualbox" do |vb|
	 vb.memory = "1024"  
end
Der erstes Schritt ist erneut die Paketverzeichnis zu akualisieren.

sudo apt-get update
Bei nächsten Schritt wird der FTP Server installiert. Das Paket lautet: pure-ftpd

#FTP Server installieren
sudo apt-get -y install pure-ftpd-common pure-ftpd
Nach der Installation kann ich die FTP-Server konfigurieren. Bei meinem Fall sieht es so aus:

sudo groupadd ftpgroup
sudo useradd -g ftpgroup -d /dev/null -s /etc ftpuser
sudo pure-pw useradd samjoy -u ftpuser -g ftpgroup -d /home/pubftp/samjoy -N 10
Nach der Änderung wird der FTP Service neu gestartet.

#FTP Server neustarten
sudo service pure-ftpd-common pure-ftpd restart
#sudo /home/pubftp/samjoy restart
Die Tastaturlayout muss man noch auf Deutsch Schweiz anpassen.

#Tastaturlayout anpassen
sudo sed -i 's/XKBLAYOUT="us"/XKBLAYOUT="ch"/g' /etc/default/locale
Am Ende wird noch eine lokale Firewall installiert und anschliessend auch konfiguriert. Dabei öffnen man den Port 22 um via SSH darauf zuzugreifen. INFO-INPUT SSH Port 22 | TELNET Port 23

#Local Firewall installieren
sudo apt-get -y install ufw gufw 
sudo ufw allow from 10.0.2.2 to any port 22
sudo ufw --force enable    
Nun ist die FTP-Part abgeschlossen.
