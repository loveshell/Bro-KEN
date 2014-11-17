##############################################
# Install Dependencies                       #
##############################################

apt-get install -y openjdk-7-jre-headless, nginx, pv, htop, socat,

##############################################
# Configure UFW Firewall                     #
##############################################

#  Allow HTTP(s)
ufw allow 80
ufw allow 443

# Allow syslog TCP/UDP in
ufw allow 514

# Allow syslog SSL in
ufw allow 1514

# Allow 
ufw allow 9200

##############################################
# Install ElasticSearch                      #
##############################################

# Add ElasticSearch Repo to Ubuntu
wget -qO - http://packages.elasticsearch.org/GPG-KEY-elasticsearch | sudo apt-key add -
cat /etc/apt/sources.list >> "deb http://packages.elasticsearch.org/elasticsearch/1.4/debian stable main"

# Update and install ElasticSearch
apt-get update
apt-get install -y elasticsearch



##############################################
# Install ElasticSearch Plugins              #
##############################################
/usr/share/elasticsearch/bin/plugin -i royrusso/elasticsearch-hq


##############################################
# Setup NXLOG                                #
##############################################

# Download NXLOG
wget http://nxlog.org/system/files/products/files/1/nxlog-ce_2.8.1248_ubuntu_1404_amd64.deb
dpkg -i nxlog-ce_2.8.1248_ubuntu_1404_amd64.deb
apt-get install -f

# Remove default NXLOG configuration & replace
rm /etc/nxlog/nxlog.conf 
cp nxlog.conf /etc/nxlog/nxlog.conf

# Create certificates for SSL (directory in config)
mkdir /etc/nxlog/ssl
openssl req -x509 -nodes -days 1095 -newkey rsa:2048 -keyout /etc/nxlog/ssl/nxlog.key -out /etc/nxlog/ssl/nxlog.crt


##############################################
# Setup Kibana3                              #
##############################################
