# azure-cli
Azure tools. (Git, Ansible, Azire Cli)

``` bash
az login
az account set --subscription sans1glbsubgeneriglob001
export RG=sans1weursgcldk8stech002
az deployment group create -g $RG --name azure-cli --template-file azure-template/template.json --parameters @azure-template/parameters.json
az vm extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --vm-name azure-cli \
  --resource-group $RG \
  --settings '{"commandToExecute":"sudo yum install -y git && sudo git clone https://github.com/alesbatalla/azure-cli.git"}'


  
az vm delete  --name azure-cli   --resource-group  $RG  -y
az disk delete --name azure-cli_OSDisk_0 --resource-group $RG  -y
az network nic delete --name azure-cli-ni --resource-group $RG
az network public-ip delete -g $RG -n azure-cli-ip
```


## Yum dependencies

sudo yum install -y git ansible epel-release
sudo yum install -y nginx

sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[azure-cli]
name=Azure CLI
baseurl=https://packages.microsoft.com/yumrepos/azure-cli
enabled=1
gpgcheck=1
gpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/azure-cli.repo'

sudo yum install -y azure-cli

## Config SSH

#stream {
#	server {
#		listen 8080;
#		proxy_pass 127.0.0.1:22;
#	}
#}
sudo echo 'stream { server { listen 8080; proxy_pass 127.0.0.1:22; } }' >>/etc/nginx/nginx.conf

sudo setsebool -P httpd_can_network_connect 1
sudo systemctl start nginx.service
sudo systemctl enable nginx



#https://serverfault.com/questions/388552/nginx-sub-domain-proxy-pass
#server_name *.hostname.com;
#
#if ($host ~* ^([0-9]+)\.hostname\.com$) {
#    set $proxyhost 192.168.56.$1;
#}
#
#proxy_pass http://$proxyhost;


#server {
#	listen80 default_server;
#	listen[::]:80 default_server;
#	server_name  "~^(?<domain>.+)\.40\.68\.101\.204\.sslip\.io$";
#	root/usr/share/nginx/html;
#
#	# Load configuration files for the default server block.
#	include /etc/nginx/default.d/*.conf;
#	location / {
#       proxy_pass http://$domain.40.68.101.204.sslip.io;              
#}
