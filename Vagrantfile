# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|
  config.vm.define "pedro" do |pedro|
    # Configuración de la máquina virtual
    pedro.vm.box = "debian/bookworm64"
    pedro.vm.network "private_network", ip: "192.168.57.103"


    # Aprovisionamiento de la máquina
    pedro.vm.provision "shell", inline: <<-SHELL
      # Actualización del sistema e instalación de paquetes
      apt-get update -y
      apt-get install -y nginx openssl

      # Crear directorios para los sitios web
      mkdir -p /var/www/pedro/html
      mkdir -p /var/www/perfectweb/html

      # Clonar el repositorio en /var/www/pedro/html
      git clone https://github.com/cloudacademy/static-website-example /var/www/pedro/html

      # Copiar los archivos de perfectweb 
      cp -r /vagrant/html/* /var/www/perfectweb/html/

      # Configuración de permisos
      chown -R www-data:www-data /var/www/pedro
      chmod -R 755 /var/www/pedro

      chown -R www-data:www-data /var/www/perfectweb
      chmod -R 755 /var/www/perfectweb

      # Copiar archivo de autenticación .htpasswd
      cp /vagrant/.htpasswd /etc/nginx/.htpasswd

      # Configuración de Nginx para 'pedro'
      cp /vagrant/pedro /etc/nginx/sites-available/pedro
      ln -sf /etc/nginx/sites-available/pedro /etc/nginx/sites-enabled/pedro

      # Configuración de Nginx para 'perfectweb'
      cp /vagrant/perfectweb /etc/nginx/sites-available/perfectweb
      ln -sf /etc/nginx/sites-available/perfectweb /etc/nginx/sites-enabled/perfectweb

      # Verificar la configuración de Nginx
      nginx -t

      # Reiniciar Nginx para aplicar los cambios
      systemctl restart nginx
    SHELL
  end
end
