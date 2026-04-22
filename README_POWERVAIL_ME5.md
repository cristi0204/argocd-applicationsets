
Install required linux pakages
```
apt install open-iscsi scsitools multipath-tools -y
```
Check available file systems
```
cat /proc/filesystems
```
Configure multipath
```
nano /etc/multipath.conf
```
with following content
```
defaults {
    user_friendly_names no
    find_multipaths greedy
    polling_interval 2
}

devices {
    device {
        vendor "DELL"
        product "PowerVault ME5224"                             
        path_grouping_policy "group_by_prio"
        path_checker "tur"
        prio "alua"
        hardware_handler "1 alua"
        failback "immediate"
    }
}
```
restart services
```
sudo systemctl enable --now iscsid
sudo systemctl enable --now multipathd
sudo systemctl restart multipathd
```
check local iscsi initiator name
```
cat /etc/iscsi/initiatorname.iscsi
```
discover iscsi targets, replace with one of controller ip's
```
sudo iscsiadm -m discovery -t sendtargets -p 10.111.111.171
```
discover all iscsi targets
```
sudo iscsiadm -m discovery -t sendtargets -p 10.200.10.13
```
and login with all of them
```
sudo iscsiadm -m node --login
```
or, just specific target in case they are not all reachable from spefic location
```
sudo iscsiadm -m node --targetname iqn.1988-11.com.dell:01.array.bc305b69b207 --portal 10.200.10.13:3260 --login
```
