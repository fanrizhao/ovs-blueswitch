# ovs-blueswitch
openvswitch for blueswitch

$ git clone https://github.com/pmundkur/ovs.git
# or download from https://github.com/pmundkur/ovs/tree/blueswitch
# attention: git clone to branch blueswitch
# unzip ovs-blueswitch.zip and mv ovs-blueswitch ovs
cd /usr/local/src/ovs
./boot.sh 
./configure --with-linux=/lib/modules/`uname -r`/build --with-debug --enable-blueswitch
make
make install
modprobe gre
modprobe openvswitch
# insmod datapath/linux/openvswitch.ko
make modules_install
mkdir -p /usr/local/etc/openvswitch
ovsdb-tool create /usr/local/etc/openvswitch/conf.db vswitchd/vswitch.ovsschema 2>/dev/null
ovsdb-server --remote=punix:/usr/local/var/run/openvswitch/db.sock      --remote=db:Open_vSwitch,Open_vSwitch,manager_options      --private-key=db:Open_vSwitch,SSL,private_key      --certificate=db:Open_vSwitch,SSL,certificate          --bootstrap-ca-cert=db:Open_vSwitch,SSL,ca_cert              --pidfile --detach
ovs-vsctl --no-wait init
ovs-vswitchd --pidfile --detach
