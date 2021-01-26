Keros 4.3.3: CPF configuration\
Since firmware 4.3.x, CPF is installed in Keros firmware\

How to configure and monitor the packet forwarder?\
The configuration is achieved in 3 steps:\

lorad configuration\
lorafwd configuration\

monitoring and auto-start configuration\
lorad configuration\

The goal of the lorad configuration is to define the frequency plan that will be used to receive packets from the end-devices, to enable the LBT feature and to configure the class B beacons.\

A few frequency plan templates are pre-installed on the gateway (under /etc/lorad/PLATFORM where PLATFORM equal to ibts, wifc, wiis or fevo): it is strongly recommended to copy the expected frequency plan templates in /etc/lorad/lorad.json file. The goal is to keep templates unmodified.\
\

Edit /etc/default/lorad and make sure that the CONFIGURATION_FILE field links to the template you previously copied.\
Edit /etc/default/lorad and make sure that DISABLE_LORAD=“no” is present in this file. If lorad is disabled consult the following page to activate the CPF:\ Keros application configuration - click here.\
Edit the template you copied to adapt it to your needs. Since the hardware between gateways is different, the configuration on each gateway differs (although it is quite similar).\

Wirnet iBTS

Wirnet iFemtoCell, iFemtoCell-evolution and Wirnet iStation

lorafwd configuration\
The goal of the lorafwd configuration is mainly to define to which LNS LoRa packets will be forwarded to.\

Edit /etc/default/lorafwd and make sure that DISABLE_LORAFWD=“no” is present in this file. If lorafwd is disabled consult the following page to activate the CPF: Keros application configuration - click here.\
Edit /etc/lorafwd.toml. This configuration file is formatted using the TOML v0.5.0 language:\
Most keys are pre-configured with correct values.\
The description of each key is directly written in the configuration file. If this configuration file has been modified, use /etc/lorafwd.toml.example as a model\
Hereunder are the keys that must be changed to chose your LNS:\
gwmp.node = “localhost”: The address of the LNS.\
gwmp.service.uplink = 20000: The uplink port of the LNS.\
gwmp.service.downlink = 20000 : The downlink port of the LNS.\
Monitoring and auto-start configuration\
By default, both lorad and lorafwd are started at boot time. If for some reason, one of the daemons stops, it will be automatically restarted by monit.\

The autostart behavior is handled by the symlinks /etc/rcU.d/S50lorad and /etc/rcU.d/S51lorafwd\.
The monitoring behavior is handled by the /etc/monit.d/lorad and /etc/monit.d/lorafwd files.\
The daemons can be started/stopped/rebooted and monitored with monit. See monit page for more details.\

Both daemons are independent. If one is stopped/restarted, the other one does not need to be stopped/restarted.\

Log managment

Both daemons generates logs in the /var/log/lora.log* files.\

The verbosity of the daemons can be increased using the EXTRA_ARGS field under /etc/default/lorad and /etc/default/lorafwd.

lorad:

NOTICE = -v: Displays start and stop traces\
INFO = -vv:\
Displays the configuration of the hardware (frequencies, bandwidth, spreading factor, antenna, LBT, …)\
Displays the number of uplinks and beacons sent.\
Displays TAI/PPS info\
…
DEBUG = -vvv: Displays the hexdump of the packets
lorafwd:\

NOTICE = -v: Displays start and stop traces
INFO = -vv:
Displays the configuration of the forwarder (gateway ID, LNS, uplink/downlink port, GWMP configuration, …)
Displays the uplinks and downlink meta-data
Displays the acknowledge / heartbeat / drop traces
…
DEBUG = -vvv: Displays the hexdump of the packets
gateway EUI
Gateway EUI is used by LNS to identify from which gateway messages come from.

When lorafwd starts, a file containing the default EUI is generated, based on information included in /tmp/board_info.json.

cat /var/run/lora/gateway-id.toml 
gateway.id = 0x7076FF0039050789
If a new EUI has been defined under /etc/lorafwd.toml, then, this new EUI is used by lorafwd. Otherwise, the default EUI is used.


Keros application configuration
Starting from version 4.3, KerOS firmware is delivered with Kerlink Common Packet Forwarder and WMC Embedded applications. These applications are disabled by default.

klk_apps_config
klk_apps_config is a small script used to configure Kerlink Common Packet Forwarder and WMC Embedded applications.
Kerlink advises to reboot the gateway after using klk_apps_config to enable/disable an application.

# klk_apps_config -h
klk_apps_config [options]

options:
-h, --help          show help
--activate-wmc      activate wmc embedded (and CPF)
--deactivate-wmc    deactivate wmc embedded (and CPF)
--activate-cpf      activate lorad and lorafwd
--deactivate-cpf    deactivate lorad and lorafwd
-s, --wmc-server    WMC server (mandatory if activating WMC)
-p, --wmc-port      WMC port (default 1194)
--lns-server        LNS server (default 172.17.0.1)
--lns-dport         LNS downlink port (default 1700)
--lns-uport         LNS uplink port (default 1700)
--loradconf         lorad configuration file name/path
             Can be:
                    - a complete file path
                    - an example file name (ex:AS923-JP.json)
This script can be used to:

activate CPF (lorad and lorafwd) only.
deactivate CPF
activate WMC embedded applications (CPF + BSCC + lora_snmp)
deactivate WMC embedded applications
It can also be used to switch to another WMC server while WMC embedded is already activated (use –activate-wmc action with another server parameter while WMC Embedded is already activated).

Script parameters details:

One of the four possible actions must be chosen (activate-wmc, deactivate-wmc, activate-cpf, deactivate-cpf)
--wmc-server: address of the WMC server to connect to (example: wmcvalidinstall.wanesy.com)
--wmc-port: port of the WMC server to connect to
--lns-server: address of the LNS server (lorafwd parameter)
--lns-dport: LNS downlink port (lorafwd parameter)
--lns-uport: LNS uplink port (lorafwd parameter)
--loradconf: this can be the complete path of the Lorad configuration you want to use or the file name of the example lorad configuration delivered for your platform. If not given the platform default will be used (EU868XXX.json)
