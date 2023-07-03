Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"   # Box do Ubuntu 20.04

  config.vm.define "nginx_vm" do |machine|
    machine.vm.hostname = "nginx_vm"

    machine.vm.network "forwarded_port", guest: 80, host: 8080

    machine.vm.synced_folder "../data", "/vagrant_data"

    machine.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y nginx
      rm /etc/nginx/sites-enabled/default
      cat << EOF > /etc/nginx/sites-available/my_site
server {
    listen 80;
    server_name localhost;

    location / {
        root /vagrant_data;
        index index.html;
    }
}
EOF
      ln -s /etc/nginx/sites-available/my_site /etc/nginx/sites-enabled/my_site
      systemctl restart nginx
    SHELL
  end
end
