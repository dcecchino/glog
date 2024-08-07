# glog vmware content pack and extractors for graylog confirmed tested on 6.x+ graylog-server written for hypervisors and appliance version of vcenter
# Note for VMware 8 and Graylog 6 installations has been tested  
Provides Graylog Dashboards for all Hypervisors, Storage performance, DVS Messages, Vmware version, Storage path failures, Host/Device Performance issues, Memory/CPU alerts, Last list of vmotions, MAC to DVS, VMware port group to hypervisor, Last login failures, Last successful logins, Last 2 hours guests attempting network sniffing, TOP LDAP users, and Vmware virtual machines recent changes by users all in a simple to use Dashboard competely customizable! To get the best benefit make sure your graylog instance is configured for syslog UDP, and make sure to use distributed switching within vmware! Have fun! Extractions using GROK, I've not had the time to change this to regex!

![image](https://github.com/dcecchino/glog/assets/17807052/5faea0fc-e406-4bf6-a753-d00704fdcbb2)


New: Cohesity Extractors and Dashboard for Backups 
New: Dell and Cisco UCS Extractions
New: VMware 7 regex extractions
New: Security Extractions 

# READ CAREFULLY (new vcenter 7 extractor should be used for vmware 7/8 only not 6.5, if you use the 6.5 extractors for a 7.0 vcenter instance your buffer processes will fill up very quickly and cause graylog to stop responding, also to install the content pack you have to have a minimum version of 6

1. Download content_pack.json and install it under System/Input Content Packs
2. Download vmware_esxi_extractors (apply to esxi input) or vmware7_extraction (version 7.0 or higher and apply to vcenter input) and import it under the System/Inputs/Manage extractors 
3. It is recommended to apply a dedicated bucket ports/syslog input for vmware to structure your data!
4. Make sure you point your syslog for both hypervisors and vcenters, start receiving your data. View the Vmware Dashboard.
5. Wait for your data to start coming in. 

# Enable high port on graylog server iptables 

```
iptables -t nat -A PREROUTING -p udp --dport 514 -j REDIRECT --to 1514
iptables -t nat -A PREROUTING -p tcp --dport 514 -j REDIRECT --to 1514
```


# Tune your esxi syslog configuration via ssh for VMware version 6.5 AND VMWARE 7.0U3 or LESS!!

```
sed -i 's/verbose/error/g' /etc/vmware/vpxa/vpxa.cfg
sed -i 's/verbose/error/g' /etc/vmware/hostd/config.xml
sed -i 's/verbose/error/g' /etc/vmware/rhttpproxy/config.xml 
sed -i 's/verbose/error/g' /etc/opt/vmware/fdm/fdm.cfg  
sed -i 's/info/error/g' /etc/vmware/hostd/probe-config.xml
sed -i 's/info/error/g' /etc/vmware/vsan/vsanperf.conf 
sed -i 's/verbose/error/g' /etc/vmware/vsan/vsanmgmt-config.xml
sed -i 's/verbose/error/g'  /etc/vmware/vsan/vsanesxcmd-config.xml    
esxcli system syslog config set --loghost='udp://update_syslog_ip_or_hostname:514'
esxcli network firewall ruleset set --ruleset-id=syslog --enabled=true
esxcli network firewall refresh
/etc/init.d/vmware-fdm restart
/etc/init.d/rhttpproxy restart
/etc/init.d/hostd restart
/etc/init.d/vpxa restart
/etc/init.d/vsantraced  restart
 /etc/init.d/vsanmgmtd  restart
 sleep 5
esxcli system syslog reload 
```
# READ CAREFULLY FOR VMWARE 7 versions greater than 7.0U3 
```
In previous releases (before ESXi 7.0U3) you may be instructed by other KB article(s) to change some settings of ESXi service "vpxa" by directly editing its configuration file (/etc/vmware/vpxa/vpxa.cfg) manually and restarting the "vpxa" service.

The settings in the database are accessible by a tool: /bin/configstorecli , changes also apply to hostd 
```



Slowly migrating to regex from Grok 


