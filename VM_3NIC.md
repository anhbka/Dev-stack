### Cài đặt Devstack với 2 NIC

#### Hướng dẫn cài đặt devstack.
- Trên một node

##### Bước 1:
- Chuẩn bị một máy Centos 7 Server 64 bit với cấu hình
```sh
Cấu hình:
 - RAM 8GB
 - HDD:100GB


Phiên bản hệ điều hành: Centos 7 Server 64 bit
 
```

- Đăng nhập bằng tài khoản root và thực hiện các lệnh sau để update bản mới nhất :

`yum -y update`

##### Bước 2:
- Đăng nhập vào OS với tài khoản root.
- Tạo và gán cấu hình sudo cho user `stack`
```sh
sudo useradd -s /bin/bash -d /opt/stack -m stack
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d
sudo su - stack

```



``` sh
[[local|localrc]]
RECLONE=False
MULTI_HOST=True
HOST_IP=172.16.91.128

OPENSTACK_VERSION=queens
CINDER_BRANCH=stable/$OPENSTACK_VERSION
CEILOMETER_BRANCH=stable/$OPENSTACK_VERSION
GLANCE_BRANCH=stable/$OPENSTACK_VERSION
HEAT_BRANCH=stable/$OPENSTACK_VERSION
HORIZON_BRANCH=stable/$OPENSTACK_VERSION
KEYSTONE_BRANCH=stable/$OPENSTACK_VERSION
NEUTRON_BRANCH=stable/$OPENSTACK_VERSION
NOVA_BRANCH=stable/$OPENSTACK_VERSION
SWIFT_BRANCH=stable/$OPENSTACK_VERSION
ZAQAR_BRANCH=stable/$OPENSTACK_VERSION
MAGNUM_BRANCH=stable/$OPENSTACK_VERSION
MURANO_BRANCH=stable/$OPENSTACK_VERSION
OCTAVIA_BRANCH=stable/$OPENSTACK_VERSION
BARBICAN_BRANCH=stable/$OPENSTACK_VERSION
BARBICAN_BRANCH=stable/$OPENSTACK_VERSION


enable_plugin neutron-lbaas https://git.openstack.org/openstack/neutron-lbaas $NEUTRON_BRANCH
enable_plugin octavia https://git.openstack.org/openstack/octavia $OCTAVIA_BRANCH

enable_plugin heat https://git.openstack.org/openstack/heat  $HEAT_BRANCH
enable_plugin heat-dashboard https://github.com/openstack/heat-dashboard.git  $HEAT_BRANCH

enable_plugin barbican https://git.openstack.org/openstack/barbican  $BARBICAN_BRANCH

enable_plugin magnum https://github.com/openstack/magnum $MAGNUM_BRANCH
enable_plugin magnum-ui https://github.com/openstack/magnum-ui $MAGNUM_BRANCH


LIBS_FROM_GIT+=,python-neutronclient,python-octaviaclient
DATABASE_PASSWORD=password
ADMIN_PASSWORD=password
SERVICE_PASSWORD=password
SERVICE_TOKEN=password
RABBIT_PASSWORD=password
LOGFILE=$DEST/logs/stack.sh.log
VERBOSE=True
LOG_COLOR=False
LOGDAYS=1

# Pre-requisites
ENABLED_SERVICES=rabbit,mysql,key

# Horizon
ENABLED_SERVICES+=,horizon
enable_service heat-dashboard

# Nova
enable_service n-api
enable_service n-cpu
enable_service n-cond
enable_service n-sch
enable_service n-api-meta
enable_service placement-api
enable_service placement-client

# Glance
enable_service g-api
enable_service g-reg

# Cinder 

ENABLED_SERVICES+=,cinder,c-sch,c-api,c-vol
CINDER_LVM_TYPE=thin
CINDER_VOLUME_CLEAR=none
VOLUME_BACKING_FILE_SIZE=40G


# Neutron
enable_service q-svc
enable_service q-agt
enable_service q-dhcp
enable_service q-l3
enable_service q-meta
enable_service q-lbaasv2

# LBaaS V2 and Octavia
enable_service octavia
enable_service o-cw
enable_service o-hm
enable_service o-hk
enable_service o-api

Q_PLUGIN=ml2
Q_ML2_TENANT_NETWORK_TYPE=vxlan

# Networking
FLOATING_RANGE=192.168.239.0/24
Q_FLOATING_ALLOCATION_POOL=start=192.168.239.150,end=192.168.239.200
PUBLIC_NETWORK_GATEWAY=192.168.239.1

# Khai bao dai mang private
IPV4_ADDRS_SAFE_TO_USE=10.10.10.0/24
NETWORK_GATEWAY=10.10.10.1

PUBLIC_INTERFACE=ens5

Q_USE_PROVIDERNET_FOR_PUBLIC=True
Q_L3_ENABLED=True
Q_USE_SECGROUP=True
OVS_PHYSICAL_BRIDGE=br-ex
PUBLIC_BRIDGE=br-ex
OVS_BRIDGE_MAPPINGS=public:br-ex

Q_ML2_PLUGIN_PATH_MTU=1454

# Setup phien ban IP se su dung
IP_VERSION=4

## Vo hieu hoa cac dich vu sau

disable_service n-net
disable_service tempest
```
