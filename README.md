# glog vmware content pack and extractors for graylog confirmed tested on graylog 3.x 
Provides Graylog Dashboards for all Hypervisors, Storage performance, DVS Messages, Vmware version, Storage path failures, Host/Device Performance issues, Memory/CPU alerts, Last list of vmotions, MAC to DVS, VMware port group to hypervisor, Last login failures, Last successful logins, Last 2 hours guests attempting network sniffing, TOP LDAP users, and Vmware virtual machines recent changes by users all in a simple to use Dashboard competely customizable!

1. Download content_pack.json and install it under System/Input Content Packs
2. Download vmware_vcenter_extractors and import it under the System/Inputs/Manage extractors 
3. Make sure you point your syslog for both hypervisors and vcenters, start receiving your data. View the Vmware Dashboard. 
4. Wait for your data to start coming in. 

# Tune your syslog configuration via ssh 
# sed -i 's/verbose/error/g' /etc/vmware/rhttpproxy/config.xml 
# sed -i 's/verbose/error/g' /etc/opt/vmware/fdm/fdm.cfg  
# sed -i 's/info/error/g' /etc/vmware/hostd/probe-config.xml
# esxcli system syslog config set --loghost='udp://yoursyslog.yourdomain.com:514'   -<change this line 
# esxcli network firewall ruleset set --ruleset-id=syslog --enabled=true
# esxcli network firewall refresh
# /etc/init.d/vmware-fdm restart
# /etc/init.d/rhttpproxy restart
# esxcli system syslog reload 
