Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  config.vm.network "forwarded_port", guest: 80, host: 8888
  config.vm.network "forwarded_port", guest: 443, host: 8443

  config.vm.provision "shell", inline: <<-SHELL
    yum update httpd
    yum install -y httpd mod_ssl policycoreutils-python

    mkdir -p /var/www/microdan.com/html /var/www/microdan.com/log
    ln -s /var/www/microdan.com/html /home/vagrant/www-content
    cp /vagrant/microdan.com.conf /etc/httpd/conf.d/
    cp /vagrant/index.html /var/www/microdan.com/html
    openssl req -new -newkey rsa:4096 -days 365 -nodes -x509 \
      -subj "/C=UA/ST=Lviv/L=Lviv/O=Microdan, LLC/CN=microdan.com" \
      -keyout /etc/ssl/microdan.key -out /etc/ssl/microdan.crt

    semanage fcontext -a -t httpd_log_t "/var/www/microdan.com/log(/.*)?"
    sudo restorecon -R -v /var/www/microdan.com/log
    systemctl start httpd
    systemctl enable httpd
  SHELL
end
