Title: Installing Openstack on Ubuntu 14.04
Slug: 
Date: 2014-05-26 18:12:39
Tags: 
Category: 
Author: 
Lang: 
Summary: How to Install Openstack on Ubuntu 14.04

username
hostname
password


HOST SET UP 


# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto p2p1
iface p2p1 inet manual
up ip address ad 0/0 dev $IFACE
up ip link set $IFACE up up link set $IFACE promisc on
down ip link set $IFACE promisc off
down ip link set $IFACE down


auto br-ex
iface br-ex inet static
address 192.168.1.11
netmask 255.255.255.0
gateway 192.168.1.254
dns-nameserver 192.168.1.254


apt-get install python-mysqldb mysql-server


root pwd = rootpwd


# /etc/mysql/my.cnf > bind address

restart mysql

apt-get install rabbitmq-server

/etc/sysctl.conf
net.ipv4.ip_forward=1   

ntp

KEYSTONE_

Install the Keystone identity service:

apt-get install keystone python-keystoneclient
Create a new MySQL database for Keystone:

mysql -u root -p
CREATE DATABASE keystone;
GRANT ALL ON keystone.* TO 'keystoneUser'@'%' IDENTIFIED BY 'keystonePass';
quit;

> Set up mysql connection 
vi /etc/keystone/keystone.conf
#and edit the connection field to show
connection = mysql://keystoneUser:keystonePass@192.168.1.11/keystone

__

export OS_SERVICE_TOKEN=T0K3N
export SERVICE_ENDPOINT="http://192.168.1.11:35357/v2.0"


keystone tenant-create --name=hq --description="HQ Tenant"

keystone user-create --name=admin --pass=admin \
   --email=tiagu.asp@gmail.com

 keystone role-create --name=admin
keystone user-role-add --user=admin --tenant=hq --role=admin

 keystone service-create --name=keystone --type=identity  --description="Keystone Identity Service"


keystone endpoint-create --service-id=SERVICE_ID_CREATED  --publicurl=http://192.168.1.11:5000/v2.0  --internalurl=http://192.168.1.11:5000/v2.0  --adminurl=http://192.168.1.11:35357/v2.0

keystone service-create --name nova --type compute --description 'Compute Service'
+-------------+----------------------------------+
|   Property  |              Value               |
+-------------+----------------------------------+
| description |         Compute Service          |
|   enabled   |               True               |
|      id     | ea3f1817b8594595a7f5e6f2184b39c1 |
|     name    |               nova               |
|     type    |             compute              |
+-------------+----------------------------------+

 keystone endpoint-create  --service-id 4f6bf83194b84523bb5add8dd9104ed7 --publicurl http://192.168.1.11:8774/v2/%\(tenant_id\)s --adminurl http://192.168.1.11:8774/v2/%\(tenant_id\)s --internalurl http://192.168.1.11:8774/v2/%\(tenant_id\)s


keystone service-create --name cinder --type volume --description 'Volume Service'
+-------------+----------------------------------+
|   Property  |              Value               |
+-------------+----------------------------------+
| description |          Volume Service          |
|   enabled   |               True               |
|      id     | e3b29a2c11004ef0b5077b0af9b07363 |
|     name    |              cinder              |
|     type    |              volume              |
+-------------+----------------------------------+

 keystone endpoint-create  --service-id 6657077de45942efbda1118dc8034b9c --publicurl http://192.168.1.11:8776/v1/%\(tenant_id\)s --adminurl http://192.168.1.11:8776/v1/%\(tenant_id\)s --internalurl http://192.168.1.11:8776/v1/%\(tenant_id\)s

keystone service-create --name glance --type image --description 'Image Service'
+-------------+----------------------------------+
|   Property  |              Value               |
+-------------+----------------------------------+
| description |          Image Service           |
|   enabled   |               True               |
|      id     | b5469e06834846309fee61de65d124e2 |
|     name    |              glance              |
|     type    |              image               |
+-------------+----------------------------------+

keystone endpoint-create --service-id d7dcfe51725c42a584c26641a53a5bbf --publicurl http://192.168.1.11:9292/v2 --adminurl http://192.168.1.11:9292/v2 --internalurl http://192.168.1.11:9292/v2

keystone user-create --name=glance --pass=GLANCE_PASS --email=tiagu.asp@gmail.com






  _GLANCE_


  apt-get install glance glance-api python-glanceclient glance-common glance-registry python-glance


Edit /etc/glance/glance-api.conf and /etc/glance/glance-registry.conf and change the [DEFAULT] section.

...
[DEFAULT]
...
# SQLAlchemy connection string for the reference implementation
# registry server. Any valid SQLAlchemy connection string is fine.
# See: http://www.sqlalchemy.org/docs/05/reference/sqlalchemy/connections.html#sqlalchemy.create_engine
sql_connection = mysql://glance:GLANCE_DBPASS@192.168.1.11/glance
...
By default, the Ubuntu packages create an SQLite database. Delete the glance.sqlite file created in the /var/lib/glance/ directory so that it does not get used by mistake.

Use the password you created to log in as root and create a glance database user:

# mysql -u root -p
CREATE DATABASE glance;
GRANT ALL PRIVILEGES ON glance.* TO 'glance'@'%' \
IDENTIFIED BY 'GLANCE_DBPASS';
GRANT ALL PRIVILEGES ON glance.* TO 'glance'@'%' \
IDENTIFIED BY 'GLANCE_DBPASS';

 keystone user-role-add --user=glance --tenant=hq --role=admin


db_sync 

PART7

http://docs.openstack.org/havana/install-guide/install/apt/content/glance-install.html




2014-05-25 22:58:48.974 2811 CRITICAL glance [-] ValueError: Tables "migrate_version" have non utf8 collation, please make sure all tables are CHARSET=utf8
The following lines should be added to /etc/my.cnf file [mysqld] section. This should fix this problem for new installations.
collation-server = utf8_general_ci
init-connect='SET NAMES utf8'
character-set-server = utf8
  2. for existing ones, I think a migration script would help. The script should scan through all tables and change charset, if it is not previously set as utf8.
  ALTER TABLE <table> CONVERT TO CHARACTER SET 'utf8';







  _NOVA_

  apt-get install nova-api nova-cert nova-common nova-compute nova-compute-kvm nova-doc nova-network nova-objectstore nova-scheduler nova-volume nova-consoleauth novnc python-nova python-novaclient

CREATE DATABASE nova;
GRANT ALL PRIVILEGES ON nova.* TO 'nova'@'localhost' \
IDENTIFIED BY 'NOVA_DBPASS';
GRANT ALL PRIVILEGES ON nova.* TO 'nova'@'%' \
IDENTIFIED BY 'NOVA_DBPASS';



_



keystone user-create --name=nova --pass=NOVA_PASS 

keystone user-role-add --user=nova --tenant=hq --role=admin



nova-manage db sync


service nova-api restart
service nova-cert restart
service nova-consoleauth restart
service nova-scheduler restart
service nova-conductor restart
service nova-novncproxy restart


http://docs.openstack.org/havana/install-guide/install/apt/content/nova-controller.html

_COMPUTE


apt-get install nova-compute-kvm python-guestfs



To also enable this override for all future kernel updates, create the file /etc/kernel/postinst.d/statoverride containing:

Select Text
1
2
3
4
5
#!/bin/sh
version="$1"
# passing the kernel version is required
[ -z "${version}" ] && exit 0
dpkg-statoverride --update --add root root 0644 /boot/vmlinuz-${version}







_CINDER

 mysql -u root -p
CREATE DATABASE cinder;
GRANT ALL PRIVILEGES ON cinder.* TO 'cinder'@'localhost' \
IDENTIFIED BY 'CINDER_DBPASS';
GRANT ALL PRIVILEGES ON cinder.* TO 'cinder'@'%' \
IDENTIFIED BY 'CINDER_DBPASS';



 keystone user-create --name=cinder --pass=CINDER_PASS 
# keystone user-role-add --user=cinder --tenant=service --role=admin






keystone endpoint-create \
  --service-id=the_service_id_above \
  --publicurl=http://192.168.1.11:8776/v1/%\(tenant_id\)s \
  --internalurl=http://192.168.1.11:8776/v1/%\(tenant_id\)s \
  --adminurl=http://192.168.1.11:8776/v1/%\(tenant_id\)s




_DASHBOARD

apt-get install memcached libapache2-mod-wsgi openstack-dashboard
apt-get remove --purge openstack-dashboard-ubuntu-theme

http://controller/horizon


_neutron


service neutron-server restart
service neutron-dhcp-agent restart
service neutron-l3-agent restart
service neutron-metadata-agent restart

_TSHOOT

CRITICAL keystone [-] ValueError: invalid literal for int() with base 10: '' > sql connection error











