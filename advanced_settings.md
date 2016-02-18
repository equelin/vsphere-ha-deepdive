# Advanced Settings

There are various types of KB articles and this KB article explains it, but let me summarize it and simplify it a bit to make it easier to digest.

There are various sorts of advanced settings, but for HA three in particular:

* das.* –> Cluster level advanced setting.
* fdm.* –> FDM host level advanced setting
* vpxd.* –> vCenter level advanced setting.


## How do you configure these?

Configuring these is typically straight forward, and most of you hopefully know this already, if not, let us go over the steps to help configuring your environment as desired.

**Cluster Level**<br>
In the Web Client: 
* Click “Hosts and Clusters”
* click your cluster object
* click the “Manage” tab
* click “Settings” and “vSphere HA”
* hit the “Edit” button

**FDM Host Level**<br>
* Open up an SSH session to your host and edit “/etc/opt/vmware/fdm/fdm.cfg”

**vCenter Level**<br>
In the Web Client: 
* Click “vCenter”
* click “vCenter Servers”
* select the appropriate vCenter Server and click the “Manage” tab
* click “Settings” and “Advanced Settings”

In this section we will primarily focus on the ones most commonly used, a full detailed list can be found in [KB 2033250](https://kb.vmware.com/kb/2033250). Please note that each bullet details the version which supports this advanced setting.

* das.maskCleanShutdownEnabled - 5.0, 5.1, 5.5
  * Whether the clean shutdown flag will default to false for an inaccessible and poweredOff VM. Enabling this option will trigger VM failover if the VM's home datastore isn't accessible when it dies or is intentionally powered off.
* das.ignoreInsufficientHbDatastore - 5.0, 5.1, 5.5, 6.0
  * Suppress the host config issue that the number of heartbeat datastores is less than das.heartbeatDsPerHost. Default value is “false”. Can be configured as “true” or “false”.
* das.heartbeatDsPerHost - 5.0, 5.1, 5.5, 6.0
  * The number of required heartbeat datastores per host. The default value is 2; value should be between 2 and 5.
* das.failuredetectiontime - 4.1 and prior
  * Number of milliseconds, timeout time, for isolation response action (with a default of 15000 milliseconds). Pre-vSphere 4.0 it was a general best practice to increase the value to 60000 when an active/standby Service Console setup was used. This is no longer needed. For a host with two Service Consoles or a secondary isolation address a failuredetection time of 15000 is recommended.
* das.isolationaddress[x] - 5.0, 5.1, 5.5, 6.0
  * IP address the ESX hosts uses to check on isolation when no heartbeats are received, where [x] = 0 ‐ 9. (see screenshot below for an example) VMware HA will use the default gateway as an isolation address and the provided value as an additional checkpoint. I recommend to add an isolation address when a secondary service console is being used for redundancy purposes.
* das.usedefaultisolationaddress - 5.0, 5.1, 5.5, 6.0
  * Value can be “true” or “false” and needs to be set to false in case the default gateway, which is the default isolation address, should not or cannot be used for this purpose. In other words, if the default gateway is a non-pingable address, set the “das.isolationaddress0” to a pingable address and disable the usage of the default gateway by setting this to “false”.
* das.isolationShutdownTimeout - 5.0, 5.1, 5.5, 6.0
  * Time in seconds to wait for a VM to become powered off after initiating a guest shutdown, before forcing a power off.
* das.allowNetwork[x] - 5.0, 5.1, 5.5
  * Enables the use of port group names to control the networks used for VMware HA, where [x] = 0 – ?. You can set the value to be ʺService Console 2ʺ or ʺManagement Networkʺ to use (only) the networks associated with those port group names in the networking configuration. In 5.5 this option is ignored when VSAN is enabled by the way!
* das.bypassNetCompatCheck - 4.1 and prior
  * Disable the “compatible network” check for HA that was introduced with ESX 3.5 Update 2. Disabling this check will enable HA to be configured in a cluster which contains hosts in different subnets, so-called incompatible networks. Default value is “false”; setting it to “true” disables the check.
* das.ignoreRedundantNetWarning - 5.0, 5.1, 5.5
  * Remove the error icon/message from your vCenter when you don’t have a redundant Service Console connection. Default value is “false”, setting it to “true” will disable the warning. HA must be reconfigured after setting the option.
* das.vmMemoryMinMB - 5.0, 5.1, 5.5
  * The minimum default slot size used for calculating failover capacity. Higher values will reserve more space for failovers. Do not confuse with “das.slotMemInMB”.
* das.slotMemInMB - 5.0, 5.1, 5.5
  * Sets the slot size for memory to the specified value. This advanced setting can be used when a virtual machine with a large memory reservation skews the slot size, as this will typically result in an artificially conservative number of available slots.
* das.vmCpuMinMHz - 5.0, 5.1, 5.5
  * The minimum default slot size used for calculating failover capacity. Higher values will reserve more space for failovers. Do not confuse with “das.slotCpuInMHz”.
* das.slotCpuInMHz - 5.0, 5.1, 5.5
  * Sets the slot size for CPU to the specified value. This advanced setting can be used when a virtual machine with a large CPU reservation skews the slot size, as this will typically result in an artificially conservative number of available slots.
* das.perHostConcurrentFailoversLimit - 5.0, 5.1, 5.5
  * By default, HA will issue up to 32 concurrent VM power-ons per host. This setting controls the maximum number of concurrent restarts on a single host. Setting a larger value will allow more VMs to be restarted concurrently but will also increase the average latency to recover as it adds more stress on the hosts and storage.
* das.config.log.maxFileNum - 5.0, 5.1, 5.5
  * Desired number of log rotations.
* das.config.log.maxFileSize - 5.0, 5.1, 5.5
  * Maximum file size in bytes of the log file.
* das.config.log.directory - 5.0, 5.1, 5.5
  * Full directory path used to store log files.
* das.maxFtVmsPerHost - 5.0, 5.1, 5.5
  * The maximum number of primary and secondary FT virtual machines that can be placed on a single host. The default value is 4.
* das.includeFTcomplianceChecks - 5.0, 5.1, 5.5
  * Controls whether vSphere Fault Tolerance compliance checks should be run as part of the cluster compliance checks. Set this option to false to avoid cluster compliance failures when Fault Tolerance is not being used in a cluster.
* das.iostatsinterval (VM Monitoring) - 5.0, 5.1, 5.5, 6.0
  * The I/O stats interval determines if any disk or network activity has occurred for the virtual machine. The default value is 120 seconds.
* das.config.fdm.deadIcmpPingInterval - 5.0, 5.1, 5.5
  * Default value is 10. ICPM pings are used to determine whether a slave host is network accessible when the FDM on that host is not connected to the master. This parameter controls the interval (expressed in seconds) between pings.
* das.config.fdm.icmpPingTimeout - 5.0, 5.1, 5.5
  * Default value is 5. Defines the time to wait in seconds for an ICMP ping reply before assuming the host being pinged is not network accessible.
* das.config.fdm.hostTimeout - 5.0, 5.1, 5.5
  * Default is 10. Controls how long a master FDM waits in seconds for a slave FDM to respond to a heartbeat before declaring the slave host not connected and initiating the workflow to determine whether the host is dead, isolated, or partitioned.
* das.config.fdm.stateLogInterval - 5.0, 5.1, 5.5
  * Default is 600. Frequency in seconds to log cluster state.
* das.config.fdm.ft.cleanupTimeout - 5.0, 5.1, 5.5
  * Default is 900. When a vSphere Fault Tolerance VM is powered on by vCenter Server, vCenter Server informs the HA master agent that it is doing so. This option controls how many seconds the HA master agent waits for the power on of the secondary VM to succeed. If the power on takes longer than this time (most likely because vCenter Server has lost contact with the host or has failed), the master agent will attempt to power on the secondary VM.
* das.config.fdm.storageVmotionCleanupTimeout - 5.0, 5.1, 5.
  * Default is 900. When a Storage vMotion is done in a HA enabled cluster using pre 5.0 hosts and the home datastore of the VM is being moved, HA may interpret the completion of the storage vmotion as a failure, and may attempt to restart the source VM. To avoid this issue, the HA master agent waits the specified number of seconds for a storage vmotion to complete. When the storage vmotion completes or the timer expires, the master will assess whether a failure occurred.
* das.config.fdm.policy.unknownStateMonitorPeriod - 5.0, 5.1, 5.5, 6.0
  * Defines the number of seconds the HA master agent waits after it detects that a VM has failed before it attempts to restart the VM.
* das.config.fdm.event.maxMasterEvents - 5.0, 5.1, 5.5
  * Default is 1000. Defines the maximum number of events cached by the master
* das.config.fdm.event.maxSlaveEvents - 5.0, 5.1, 5.5
  * Default is 600. Defines the maximum number of events cached by a slave.

That is a long list of advanced settings indeed, and hopefully no one is planning to try them all out on a single cluster, or even on multiple clusters. Avoid using advanced settings as much as possible as it definitely leads to increased complexity, and often to more down time rather than less.
