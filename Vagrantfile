Vagrant.configure("2") do |config|
  config.vm.define "ismael" do |ismael|
    ismael.vm.box = "debian/bookworm64"
    ismael.vm.network "private_network", ip: "192.168.57.102"
    ismael.vm.provision "shell", inline: <<-SHELL
      apt update
      apt install -y nginx git vsftpd
      

      #Creando primera web git
      mkdir -p /var/www/ismael/html
      git clone https://github.com/cloudacademy/static-website-example /var/www/ismael/html
      chown -R www-data:www-data /var/www/ismael/html
      chmod -R 755 /var/www/ismael
      cp -v /vagrant/ismael-sites /etc/nginx/sites-available/ismael
      ln -s /etc/nginx/sites-available/ismael /etc/nginx/sites-enabled/

     

      # Creando segunda web ftp
      mkdir -p /var/www/segunda/html
      useradd -d /var/www/segunda/html -m ftpuser
      echo "ftpuser:vagrant" | chpasswd
      chown -R ftpuser:ftpuser /var/www/segunda/html
      chmod -R 755 /var/www/segunda
      cp -v /vagrant/segunda-sites /etc/nginx/sites-available/segunda
      ln -s /etc/nginx/sites-available/segunda /etc/nginx/sites-enabled/

      # Configuración de SSL y vsftpd
      openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt -subj "/C=US/ST=State/L=City/O=Organization/OU=OrgUnit/CN=example.com"
      cp -v /vagrant/vsftpd.conf /etc/vsftpd.conf

      systemctl restart vsftpd
      systemctl restart nginx
      systemctl status nginx
    SHELL
  end
end
