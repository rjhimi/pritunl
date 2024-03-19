sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
##
vi /etc/yum.repos.d/mongodb-org-4.4.repo
##
[mongodb-org-4.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.4.asc
##
sudo dnf install mongodb-org -y
sudo systemctl enable --now mongod
vi /etc/yum.repos.d/pritunl.repo
##
[pritunl]
name=Pritunl Repository
baseurl=https://repo.pritunl.com/stable/yum/oraclelinux/8/
aseurl=https://repo.pritunl.com/stable/yum/almalinux/8/
gpgcheck=1
enabled=1
gpgkey=https://raw.githubusercontent.com/pritunl/pgp/master/pritunl_repo_pub.asc
##
sudo yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo yum -y update
sudo yum -y --allowerasing install pritunl-openvpn
sudo yum -y install pritunl mongodb-org
sudo systemctl enable mongod pritunl
sudo systemctl start mongod pritunl
firewall-cmd --permanent --zone=public --add-port=80/tcp
firewall-cmd --permanent --zone=public --add-port=443/tcp
firewall-cmd --permanent --zone=public --add-port=4444/tcp
