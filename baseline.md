# FortiGate baseline (2026-02-01, FortiOS 7.6.6)

- [FortiGate baseline (2026-02-01, FortiOS 7.6.6)](#fortigate-baseline-2026-02-01-fortios-766)
  - [Checks](#checks)
    - [Check and clear error logs](#check-and-clear-error-logs)
    - [Make sure FortiGuard services are up-to-date](#make-sure-fortiguard-services-are-up-to-date)
  - [Global settings](#global-settings)
    - [Activate Private Data Encryption](#activate-private-data-encryption)
    - [Automatically check the disk after an ungraceful shutdown](#automatically-check-the-disk-after-an-ungraceful-shutdown)
    - [Create configuration revisions on logout](#create-configuration-revisions-on-logout)
    - [Configure reliable DNS servers](#configure-reliable-dns-servers)
    - [Configure a reliable time source](#configure-a-reliable-time-source)
    - [Configure the correct timezone](#configure-the-correct-timezone)
    - [Enable FortiGuard AMQP subscription notifications](#enable-fortiguard-amqp-subscription-notifications)
    - [Enable loading of static artifacts from a CDN for the FortiGate GUI](#enable-loading-of-static-artifacts-from-a-cdn-for-the-fortigate-gui)
    - [Configure a password policy](#configure-a-password-policy)
    - [Increased administrator lockout duration](#increased-administrator-lockout-duration)
    - [The timeout for administrator sessions must be set to a reasonable value](#the-timeout-for-administrator-sessions-must-be-set-to-a-reasonable-value)
    - [Enable the Internet Services Database (ISDB) cache](#enable-the-internet-services-database-isdb-cache)
  - [High availability](#high-availability)
    - [Improve FortiGate FGCP/HA failover behaviour](#improve-fortigate-fgcpha-failover-behaviour)
    - [FortiGate FGCP/HA clusters should monitor interfaces](#fortigate-fgcpha-clusters-should-monitor-interfaces)
    - [Remote link failover for FortiGate FGCP/HA](#remote-link-failover-for-fortigate-fgcpha)
    - [Configure VLAN monitoring of an FGCP/HA cluster](#configure-vlan-monitoring-of-an-fgcpha-cluster)
  - [Logging and information gathering](#logging-and-information-gathering)
    - [Log CLI commands](#log-cli-commands)
    - [Monitor the bandwidth of relevant interfaces](#monitor-the-bandwidth-of-relevant-interfaces)
    - [Enable device detection on relevant interfaces](#enable-device-detection-on-relevant-interfaces)
    - [Log more and include more information in logs](#log-more-and-include-more-information-in-logs)
    - [Enable logging to disk and increase the maximum age](#enable-logging-to-disk-and-increase-the-maximum-age)
  - [Device and network security](#device-and-network-security)
    - [Disable and clear unused interfaces](#disable-and-clear-unused-interfaces)
    - [WAN-facing interfaces should not have management services enabled](#wan-facing-interfaces-should-not-have-management-services-enabled)
    - [Disable unsafe management services on interfaces and globally](#disable-unsafe-management-services-on-interfaces-and-globally)
    - [Use local-in policies to restrict access to management services](#use-local-in-policies-to-restrict-access-to-management-services)
    - [Configure and use threat feeds for firewall and local-in policies](#configure-and-use-threat-feeds-for-firewall-and-local-in-policies)
    - [Administrator users should use Multi-Factor Authentication (MFA)](#administrator-users-should-use-multi-factor-authentication-mfa)
    - [Use Internet Service Database (ISDB) objects for firewall and local-in policies](#use-internet-service-database-isdb-objects-for-firewall-and-local-in-policies)
    - [Configure trusted hosts for all administrators](#configure-trusted-hosts-for-all-administrators)
    - [Create a new administrator user and delete the system default admin](#create-a-new-administrator-user-and-delete-the-system-default-admin)
    - [Disable FortiCloud SSO](#disable-forticloud-sso)
    - [Delete unused DHCP servers](#delete-unused-dhcp-servers)
    - [Stronger encryption for FortiGate and stronger Diffie-Hellman](#stronger-encryption-for-fortigate-and-stronger-diffie-hellman)
    - [Disable static TLS keys for FortiGate-terminated traffic](#disable-static-tls-keys-for-fortigate-terminated-traffic)
    - [USB auto-install of configurations and firmware should be disabled](#usb-auto-install-of-configurations-and-firmware-should-be-disabled)
  - [Networking](#networking)
    - [Configure SD-WAN](#configure-sd-wan)
    - [Put interfaces in zones](#put-interfaces-in-zones)
    - [RFC1918 IP address space should be routed into a blackhole](#rfc1918-ip-address-space-should-be-routed-into-a-blackhole)
  - [Security profiles](#security-profiles)
    - [SSL/SSH profiles should block unsupported SSL/SSH communication](#sslssh-profiles-should-block-unsupported-sslssh-communication)
    - [Deep inspection SSL/SSH profiles should have a minimum SSL/TLS version of 1.2](#deep-inspection-sslssh-profiles-should-have-a-minimum-ssltls-version-of-12)
    - [Antivirus profiles should use the Outbreak Prevention database and quarantine files](#antivirus-profiles-should-use-the-outbreak-prevention-database-and-quarantine-files)
    - [Antivirus should use the extreme database if available](#antivirus-should-use-the-extreme-database-if-available)
    - [Antivirus should use machine learning to better detect malware](#antivirus-should-use-machine-learning-to-better-detect-malware)
    - [Intrusion Prevention System profiles should block more signatures,  malicious traffic, and log packets](#intrusion-prevention-system-profiles-should-block-more-signatures--malicious-traffic-and-log-packets)
    - [DNS filter profiles should redirect Command \& Control requests to the block portal](#dns-filter-profiles-should-redirect-command--control-requests-to-the-block-portal)
    - [DNS filters should log all queries and responses](#dns-filters-should-log-all-queries-and-responses)
    - [Application Control profiles should use Network Protocol Enforcement](#application-control-profiles-should-use-network-protocol-enforcement)
    - [Application Control profiles should block applications communicating on non-default ports](#application-control-profiles-should-block-applications-communicating-on-non-default-ports)
    - [Web filter profiles should block invalid URLs and rate URLs by domain and IP address](#web-filter-profiles-should-block-invalid-urls-and-rate-urls-by-domain-and-ip-address)
    - [Web filter profiles should log more information](#web-filter-profiles-should-log-more-information)
    - [Enable and use FortiGate Cloud Sandbox](#enable-and-use-fortigate-cloud-sandbox)


## Checks
### Check and clear error logs
Due to various reasons, mainly upgrades, errors can appear in the configuration, and they might impact production. Checking, fixing, and clearing the error logs should be done after every upgrade or configuration restore.

``` 
diagnose debug config-error-log read
diagnose debug config-error-log clear
```

### Make sure FortiGuard services are up-to-date
Antivirus and IPS signatures, certificate bundles, ISDB versions, etc., should be up-to-date. If not, an update should be triggered, which takes a few minutes to complete.  
**Note:** Signatures will only update if the associated profiles are used.

``` 
execute update-now
diagnose autoupdate versions
```

## Global settings
### Activate Private Data Encryption
Private Data Encryption (PDE) replaces the predefined encryption key with a randomly generated one. Enabling this feature greatly increases the security of the configuration, because passwords cannot be decrypted without the randomly generated key.

**Note:** Read about how FortiManager interacts with FortiGates that have PDE enabled.

[https://docs.fortinet.com/document/fortigate/7.6.5/fortios-release-notes/191180/fortimanager-support-for-updated-fortios-private-data-encryption-key](https://docs.fortinet.com/document/fortigate/7.6.5/fortios-release-notes/191180/fortimanager-support-for-updated-fortios-private-data-encryption-key)

``` 
config system global
    set private-data-encryption enable
end
```

### Automatically check the disk after an ungraceful shutdown
If a FortiGate has a disk, it requires a disk check if it was shut down in an ungraceful manner. If not done automatically, this must be done manually and requires a reboot.

``` 
config system global
    set autorun-log-fsck enable
end
```

### Create configuration revisions on logout
Configuration revisions can be automatically created upon a logout. This can help with tracking down changes.

``` 
config system global
    set revision-backup-on-logout enable
end
```

### Configure reliable DNS servers
By default, FortiGates use the FortiGuard DNS servers, which are unreliable. FortiGates should either use an internal DNS server or a reliable public DNS server.  
**Note:** The FortiGuard DNS servers use DNS over TCP, so the protocol might need to be changed.

``` 
config system dns
    set primary <###PLACEHOLDER_DNS###>  
    set secondary <###PLACEHOLDER_DNS###>  
    set protocol <###PLACEHOLDER_DNS-PROTOCOL###>  
end
```

### Configure a reliable time source
The date and time should be consistent in an environment, so every device should use a reliable time source. FortiGates use the public FortiGuard NTP servers, but a different, maybe internal source, is preferred.

``` 
config system ntp
    set ntpsync enable
    set type custom
    config ntpserver
        edit 0
            set server "<###PLACEHOLDER_NTP###>"  
        next
        edit 0
            set server "<###PLACEHOLDER_NTP###>"  
        next
    end
end
```

In situations where no NTP server is available, but the correct date and time are important, this information can be set manually.

``` 
execute date <yyyy-mm-dd>  
execute time <hh:mm:ss>  
```

### Configure the correct timezone
A correct timezone is important for log correlation.

``` 
config system global
    set timezone <###PLACEHOLDER_TIMEZONE###>  
end
```

### Enable FortiGuard AMQP subscription notifications
AMQP delivers real-time update notifications to FortiGate devices, including license alerts and database updates.

``` 
config system fortiguard
    set subscribe-update-notification enable
end
```

If AMQP subscription notifications are enabled, previously enabled persistent connections (only available on select FortiGate models) should be disabled.

``` 
config system fortiguard
    set persistent-connection disable
end
```

### Enable loading of static artifacts from a CDN for the FortiGate GUI
Loading static GUI artifacts from a CDN instead of the FortiGate can improve GUI performance for the administrator if working remotely. If the CDN connection is lost, files are served by the FortiGate.

``` 
config system global
    set gui-cdn-usage enable
end
```

### Configure a password policy
An enabled password policy is a default starting with 7.6.5, but the default policy should be made stronger by changing at least the minimum length from 12 to 24, and password reuse should be disabled.  
After enabling the password policy, if the current password does not fit the policy, the password needs to be changed on the next login.

``` 
config system password-policy
    set status enable
    set minimum-length 24
    set min-lower-case-letter 1
    set min-upper-case-letter 1
    set min-non-alphanumeric 1
    set min-number 1
    set reuse-password disable
end
```

### Increased administrator lockout duration
By default, an administrator is locked out after 3 failed attempts and for 60 seconds. The duration of the lockout should be increased to make brute-force attempts less appealing and successful.

``` 
config system global
    set admin-lockout-duration 600
end
```

### The timeout for administrator sessions must be set to a reasonable value
The default value for the timeout of administrators is 5 minutes. This value can be changed to something higher, but it is not recommended to go higher than 10 minutes.

``` 
config system global
    set admintimeout 10
end
```

### Enable the Internet Services Database (ISDB) cache
The ISDB holds dynamically updated objects for various services, like FortiGuard, Amazon AWS, Microsoft Azure, etc.  
Using the ISDB cache can increase lookup performance, because not the entire ISDB needs to be queried if the result is in a cache.

``` 
config system settings
    set internet-service-database-cache enable
end
```

## High availability
### Improve FortiGate FGCP/HA failover behaviour
By default, FortiGate HA clusters:

* Do not synchronize all types of sessions, and do not perform failovers in some situations.  
* Do not perform a failover if conserve mode (default 88% of RAM usage) is triggered.  
* Do not perform a failover if an SSD failure happens
* Ignore the uptime as a point for the primary election if the difference between the members is less than 300 seconds
FortiGate HA clusters should:

* Synchronize TCP, SCTP, UDP, ICMP, expectation and SIP ALG sessions
* Perform a failover before conserve mode is triggered (86% of RAM usage)  
* Wait 60 minutes after a failover due to RAM usage was triggered before performing another failover
* Perform a failover if an SSD failure happens on devices with a disk
* Should only ignore the uptime as a point for the primary election if the difference is less than 60 seconds
  * If the uptime difference is less than 300 seconds, the uptime is ignored for primary election, and the priority, which is 128 by default, becomes the next important value, and after that, the serial number.  
  * Reducing `ha-uptime-diff-margin` is done because during an upgrade, it is very possible that after the initial failover to the secondary, the former primary, after it is done upgrading, comes up before the default 300-second timer is over, leading to a failback, reinstating the former primary as the primary member.  
  * This behaviour is probably undesired in most deployments, and after the initial failover to the secondary, this state should be kept.  
  * If a failback is desired, it is recommended to use a higher priority on one FortiGate and enable the `override` setting.

``` 
config system ha
    set session-pickup enable
    set session-pickup-connectionless enable
    set session-pickup-expectation enable
    set memory-based-failover enable
    set memory-failover-threshold 86
    set memory-failover-flip-timeout 60
    set ssd-failover enable
    set ha-uptime-diff-margin 60
end
```

### FortiGate FGCP/HA clusters should monitor interfaces
Monitored interfaces are a determining factor for the primary election. By default, a single failed monitored interface causes a failover. Multiple interfaces can be selected.  
LAG interfaces can be selected, but also the individual members of a LAG. Both options are valid, and which one to use is mainly a question of whether you value bandwidth (fail over on a single failed member) over total failovers (monitor individual members). Setting the `min-links` value for the specified LAG under `config system interface` to 1 can also create the same behaviour as selecting individual members.

``` 
config system ha
    set monitor "<###PLACEHOLDER_INTF###>"  
end
```

### Remote link failover for FortiGate FGCP/HA
Using link monitors, or SLAs with SD-WAN, a FortiGate HA cluster can monitor a remote destination and perform a failover if the destination is not reachable from one member, in the hope that the other member can.

First, a link monitor needs to be created that, at a minimum, needs a source interface and a destination, called a pingserver, to monitor. The default protocol is ping, but this can be changed.  
Updating of cascade interfaces and routes will be disabled, because this can have unintended consequences.

After that, the interface needs to be added as a `pingserver-monitor-interface` in the HA configuration. If SD-WAN with an SLA is used, no link monitor needs to be created, and the SD-WAN member can be immediately set.  
`pingserver-flip-timeout` is the value, in minutes, during which no failover can happen because of a monitoring failure. This prevents flapping of the status and unnecessary and disruptive failovers.

The status of the pingserver can be checked using `get system ha status` on the CLI in the `PINGSRV stats` section. It is normal that the current secondary shows a state of `N/A` for the configured pingserver and that a HA health status message appears, because the secondary has no routing capabilities for this type of traffic.

With default settings, a failover will be performed as soon as a single remote destination is not reachable, but this can be changed using a combination of the `ha-priority` and `pingserver-failover-threshold` settings. If the combined priority of all pingservers is above the threshold, a failure occurs.

With default pingserver settings, `override` enabled, and if one unit has a higher priority after the flip timeout expires, the former primary will become the primary again, i.e. a failback happens. This behaviour can be changed by disabling `pingserver-secondary-force-reset`. With this disabled, the new primary will stay the primary until another election-causing event (manual failover, failed monitored interface, pingserver failure, etc.) happens.

``` 
config system link-monitor
    edit "<###PLACEHOLDER_LINK-MONITOR-NAME###>"  
        set srcintf <###PLACEHOLDER_INTF###>  
        set server <###PLACEHOLDER_DESTINATION###>  
        set protocol <###PLACEHOLDER_PROTOCOL###>  
        set ha-priority <###PLACEHOLDER_PRIORITY###>  
        set update-cascade-interface disable
        set update-static-route disable
        set update-policy-route disable
    next
end
config system ha
    set pingserver-monitor-interface <###PLACEHOLDER_INTF###>  
    set pingserver-failover-threshold <###PLACEHOLDER_THRESHOLD###>  
    set pingserver-flip-timeout <###PLACEHOLDER_MINUTES###>  
    set pingserver-secondary-force-reset disable
end
```

### Configure VLAN monitoring of an FGCP/HA cluster
If a FortiGate HA cluster has VLAN interfaces, the communication between the units, using the VLAN interfaces, over the switching infrastructure can be tested. If the communication is not successful, signalling a misconfiguration in the switching infrastructure, a log message is created.

``` 
config system ha-monitor
    set monitor-vlan enable
end
```

The status can be checked using:

``` 
diagnose debug reset
diagnose sys ha vlan-hb-monitor
diagnose debug enable
```

The status can also be debugged live.

``` 
diagnose debug reset
diagnose debug application hamonitord -1
diagnose debug enable
```

## Logging and information gathering
### Log CLI commands
Enabling the logging of CLI commands (diagnose, get, execute, config) can be useful for audit purposes as well as during incidents. These logs show up under Systems Events.

**Note:** If this option is enabled, credentials entered on some options of `diagnose test authserver`, including `radius`, `radius-direct`, `ldap` and `tacacs+-direct`, are visible in the generated logs as plaintext.

``` 
config system global
    set cli-audit-log enable
end
```

### Monitor the bandwidth of relevant interfaces
By default, there is no monitoring of the bandwidth usage of interfaces. Enabling the monitoring allows for up to 24 hours of statistics to be collected. Every relevant interface should have this feature enabled.

``` 
config system interface
    edit "<###PLACEHOLDER_INTF###>"  
        set monitor-bandwidth enable
    next
end
```

The usage can be checked using the “Interface Bandwidth” widget on the Dashboard.

### Enable device detection on relevant interfaces
FortiGates can detect some information about devices from various pieces of information (DHCP, ARP, MAC address, etc.) that can aid in correlating logs with devices.  

**Note:** If the device database grows to an enormous size, it can negatively impact the device database itself (slow loading, not loading at all, database corruption, etc.), so if lots of total devices are expected, it should not be enabled on interfaces where this information is not relevant.

``` 
config system interface
    edit "<###PLACEHOLDER_INTF###>"  
        set device-identification enable
    next
end
```

### Log more and include more information in logs
* Log the implicit deny policy
* Log local-in and local-out traffic
* Extend logs with more information
* Resolve IPs and ports if possible
* Log API actions
* Add zone names to logs
* Make sure local traffic is logged

``` 
config log setting
    set fwpolicy-implicit-log enable
    set local-in-allow enable
    set local-in-deny-unicast enable
    set local-in-deny-broadcast enable
    set local-out enable
    set extended-log enable
    set extended-utm-log enable
    set resolve-ip enable
    set resolve-port enable
    set rest-api-set enable
    set rest-api-get enable
    set zone-name enable
end
config log memory filter
    set local-traffic enable
end
```

### Enable logging to disk and increase the maximum age
FortiGates with a disk should save logs to it, and the maximum age should be increased to 0, meaning indefinite (old logs get deleted first).

``` 
config log disk setting
    set status enable
    set maximum-log-age 0
end
```

## Device and network security
### Disable and clear unused interfaces
Interfaces that are not being used should be disabled, have no IP address, and have no management services enabled.

``` 
config system interface
    edit "<###PLACEHOLDER_DISABLED-INTF###>"  
        set status down
        unset ip
        unset allowaccess
    next
end
```

### WAN-facing interfaces should not have management services enabled
No management services should be enabled on any WAN-facing interfaces. This is a security risk. If management access from an external source is required, this should be done via a VPN.

``` 
config system interface
    edit "<###PLACEHOLDER_WAN-INTF###>"  
        unselect allowaccess http https ssh snmp telnet fabric
    next
end
```

### Disable unsafe management services on interfaces and globally
If HTTPS is enabled via the GUI, HTTP also gets enabled. While a redirect exists, the HTTP service should not be listening at all.  
Telnet should never be active.

``` 
config system interface
    edit "<###PLACEHOLDER_INTF###>"  
        unselect allowaccess http telnet
    next
end
config system global
    set admin-telnet disable
end
```

### Use local-in policies to restrict access to management services
Local-in policies handle traffic to the FortiGate itself. Management services, like HTTPS and SSH, should only be allowed from specific sources.  
For this to work, an address group object should be created that includes the relevant, privileged sources that are allowed to access the FortiGate management.

Two local-in policies will also be created:

1. Allow the created address group object to use the management services
   1. This policy will also have virtual patching enabled to further enhance security
2. Deny all other traffic to the management services

**Important points:**

* It is recommended to first apply the local-in policies to either HTTPS or SSH management. If an error occurs, the other service can still be used to edit the policy. If access works, the other service can be added without worry.  
* SNMP is considered a management service, so the SNMP polling device in the environment needs to be covered by a local-in policy if you restrict access using this method, which is recommended.

``` 
config firewall address
    edit "<###PLACEHOLDER-ADDRESS-NAME###>"  
        set subnet <###PLACEHOLDER-ADDRESS###>  
    next
end
config firewall addrgrp
    edit "<###PLACEHOLDER-ADDRESS-GROUP###>"  
        set member "<###PLACEHOLDER-ADDRESS-NAME###>"  
    next
end
config firewall local-in-policy
    edit 0
        set intf "any"  
        set srcaddr "<###PLACEHOLDER-ADDRESS-GROUP###>"  
        set dstaddr "all"  
        set action accept
        set service "<###PLACEHOLDER_HTTPS###>" "<###PLACEHOLDER_SSH###>" "<###PLACEHOLDER-SNMP###>"  
        set schedule "always"  
        set comment "ALLOW PRIVILEGED NETWORKS TO ACCESS MANAGEMENT SERVICES"  
        set virtual-patch enable
    next
    edit 0
        set intf "any"  
        set srcaddr "all"  
        set dstaddr "all"  
        set action deny
        set service "<###PLACEHOLDER_HTTPS###>" "<###PLACEHOLDER_SSH###>" "<###PLACEHOLDER-SNMP###>"  
        set schedule "always"  
        set comment "DENY ACCESS TO MANAGEMENT SERVICES"  
    next
end
```

**Note:** Starting with 7.6.1, FortiGates already come with a default local-in policy that blocks the Malicious-Malicious.Server, Tor-Exit.Node, and Tor-Relay.Node ISDB objects.

### Configure and use threat feeds for firewall and local-in policies
Threat feeds are regularly downloaded lists that include information like IPs, which can be used in firewall and local-in policies to block communication from and to malicious IPs.

For this baseline, a few public threat feeds are given along with a more full-featured configuration.

**Important points:**

* The policies using these threat feeds should be placed as high up in the policy lists as is reasonable, to block as soon as possible.  
* The local-in policy denies all attempts from the entries in the threat feeds to contact the FortiGate itself, which includes protocols like IKE/IPsec, not just management services.

``` 
config system external-resource
    edit "<###PLACEHOLDER_TF-EMERGINGTHREAT-BLOCK-IPs###>"  
        set type address
        set resource "https://rules.emergingthreats.net/fwrules/emerging-Block-IPs.txt"  
    next
    edit "<###PLACEHOLDER_TF-EMERGINGTHREAT-COMPROMISED-IPs###>"  
        set type address
        set resource "https://rules.emergingthreats.net/blockrules/compromised-ips.txt"  
    next
    edit "<###PLACEHOLDER_TF-GREYNOISE###>"  
        set type address
        set resource "https://api.greynoise.io/v3/tags/ed943e41-bce1-4aae-9173-4ae36f841700/ips?format=txt&token=YNsG9pxSQRK1MFYqMVPfVw"  
    next
end
config firewall policy
    edit 0
        set name "BLOCK TRAFFIC TO BAD THREAT FEEDS"  
        set srcintf "any"  
        set dstintf "<###PLACEHOLDER_WAN-INTF###>"  
        set srcaddr "all"  
        set dstaddr "<###PLACEHOLDER_TF-EMERGINGTHREAT-BLOCK-IPs###>" "<###PLACEHOLDER_TF-EMERGINGTHREAT-COMPROMISED-IPs###>" "<###PLACEHOLDER_TF-GREYNOISE###>"  
        set schedule "always"  
        set service "ALL"  
        set logtraffic all
    next
    edit 0
        set name "BLOCK THREAT FEEDS TO ANY"  
        set srcintf "<###PLACEHOLDER_WAN-INTF###>"  
        set dstintf "any"  
        set srcaddr "<###PLACEHOLDER_TF-EMERGINGTHREAT-BLOCK-IPs###>" "<###PLACEHOLDER_TF-EMERGINGTHREAT-COMPROMISED-IPs###>" "<###PLACEHOLDER_TF-GREYNOISE###>"  
        set dstaddr "all"  
        set schedule "always"  
        set service "ALL"  
        set logtraffic all
    next
end
config firewall local-in-policy
    edit 0
        set comments "BLOCK BAD THREAT FEEDS TO FORTIGATE"  
        set intf "any"  
        set srcaddr "<###PLACEHOLDER_TF-EMERGINGTHREAT-BLOCK-IPs###>" "<###PLACEHOLDER_TF-EMERGINGTHREAT-COMPROMISED-IPs###>" "<###PLACEHOLDER_TF-GREYNOISE###>"  
        set dstaddr "all"  
        set schedule "always"  
        set service "ALL"  
    next
end
```

### Administrator users should use Multi-Factor Authentication (MFA)

MFA should be used for every administrator user. There are various methods to achieve this:

* FortiToken: Every FortiGate comes with 2 free mobile FortiTokens that can be used
* SAML SSO: SAML is an easy way to enable MFA for all types of authentication on a FortiGate, and you probably already have an MFA service
  * [Microsoft Entra ID](https://community.fortinet.com/t5/FortiGate/Technical-Tip-Configuring-SAML-SSO-login-for-FortiGate/ta-p/194656)  
  * [Cisco Duo](https://duo.com/docs/sso-fortigate-admin)  
  * [Okta](https://community.fortinet.com/t5/FortiGate/Technical-Tip-Configuring-SAML-SSO-login-for-FortiGate/ta-p/196181)  
  * Etc.  
* RADIUS-backed: Your RADIUS server may have the possibility to provide MFA using additional configuration
* SMS: While not recommended, due to being relatively insecure, it is a viable method
* Email: While not recommended, due to being relatively insecure, it is a viable method
* [Free FortiToken/FortiIdentity Cloud trial](https://docs.fortinet.com/document/fortigate/7.4.0/administration-guide/66318/enable-the-fortitoken-cloud-free-trial-directly-from-the-fortigate-new): You can use a 1-month free trial of FortiToken/FortiIdentity Cloud.

**Note:** It might be a good idea to create a break-glass administrator account with an extremely long and complex password (64+ characters), very limited trusted hosts (or only being able to log in using the console) and without MFA.

### Use Internet Service Database (ISDB) objects for firewall and local-in policies
The ISDB holds dynamically updated objects for various services, like FortiGuard, Amazon AWS, Microsoft Azure, etc., but it also includes malicious and potentially unwanted objects, like VPN, command and control, and malicious servers.

These ISDB objects can be grouped and used to block communication from and to the destinations.

**Important points:**

* The policies using these threat feeds should be placed as high up in the policy lists as is reasonable, to block as soon as possible.  
* The local-in policy denies all attempts from the entries in the threat feeds to contact the FortiGate itself, which includes protocols like IKE/IPsec, not just management services.

For this baseline, a few ISDB objects are given along with a more full-featured configuration.

``` 
config firewall internet-service-group
    edit "<###PLACEHOLDER_BAD-ISDB-SOURCE###>"  
        set direction source
        set member "Botnet-C&C.Server" "Hosting-Bulletproof.Hosting" "Malicious-Malicious.Server" "Phishing-Phishing.Server" "Proxy-Proxy.Server" "Spam-Spamming.Server" "Tor-Exit.Node" "Tor-Relay.Node" "VPN-Anonymous.VPN"  
    next
    edit "<###PLACEHOLDER_BAD-ISDB-DESTINATION###>"  
        set direction destination
        set member "Blockchain-Crypto.Mining.Pool" "Botnet-C&C.Server" "Hosting-Bulletproof.Hosting" "Malicious-Malicious.Server" "Phishing-Phishing.Server" "Proxy-Proxy.Server" "Spam-Spamming.Server" "Tor-Exit.Node" "Tor-Relay.Node" "VPN-Anonymous.VPN"  
    next
end
config firewall policy
    edit 0
        set name "BLOCK TRAFFIC TO BAD ISDB"  
        set srcintf "any"  
        set dstintf "<###PLACEHOLDER_WAN-INTF###>"  
        set srcaddr "all"  
        set internet-service enable
        set internet-service-group "<###PLACEHOLDER_BAD-ISDB-DESTINATION###>"  
        set schedule "always"  
        set logtraffic all
    next
    edit 0
        set name "BLOCK BAD ISDB TO ANY"  
        set srcintf "<###PLACEHOLDER_WAN-INTF###>"  
        set dstintf "any"  
        set dstaddr "all"  
        set internet-service-src enable
        set internet-service-src-group "<###PLACEHOLDER_BAD-ISDB-SOURCE###>"  
        set schedule "always"  
        set service "ALL"  
        set logtraffic all
    next
end
config firewall local-in-policy
    edit 0
        set comments "BLOCK ISDB SOURCES TO FORTIGATE"  
        set intf "any"  
        set internet-service-src enable
        set internet-service-src-group "<###PLACEHOLDER_BAD-ISDB-SOURCE###>"  
        set dstaddr "all"  
        set schedule "always"  
        set service "ALL"  
    next
end
```

### Configure trusted hosts for all administrators
Trusted hosts restrict who is allowed to log in with an administrator account. Trusted hosts are similar to local-in policies for management services, but are checked after local-in policies and are more granular, because trusted hosts are set per administrator.  
While trusted hosts, if set on all administrators, do the same thing as local-in policies (in such a situation, an implicit local-in policy gets created), trusted hosts are prone to misconfiguration, because a single administrator without trusted hosts, and no local-in policies can open up the management services to all sources.  
**Up to a maximum of 10 trusted host entries can be added per administrator.**

**Trusted hosts should always be used as a supplement to local-in policies and never standalone. Local-in policies are number 1 and a must.**

``` 
config system admin
    edit "<###PLACEHOLDER_ADMIN-NAME###>"  
        set trusthost1 <###PLACEHOLDER_ADDRESS###>  
        set trusthost2 <###PLACEHOLDER_ADDRESS###>  
    next
end
```

### Create a new administrator user and delete the system default admin
The admin user is available on all FortiGate devices, so it is an easy target for brute force attempts. A new administrator user should be created, and once proper production use is confirmed, the default “admin” user should be deleted.

``` 
config system admin
    edit "<###PLACEHOLDER_ADMIN-NAME###>"  
        set vdom "root"  
        set trusthost1 <###PLACEHOLDER_TRUSTED-HOST###>  
        set accprofile "super_admin"  
        set password <###PLACEHOLDER_PASSWORD###>  
    next
end
```

**Attention:** Log in as the new administrator and delete the default admin user. You will need to enter the password of the currently logged-in administrator to do so.

``` 
config system admin
    delete admin
end
```

### Disable FortiCloud SSO
FortiCloud SSO for administrator logins is a potential security vulnerability, already was in the past and should therefore be disabled.

``` 
config system global
    set admin-forticloud-sso-login disable
end
```

### Delete unused DHCP servers
By default, FortiGates come with some DHCP servers to make provisioning easier. If they are not used, these DHCP servers should be deleted.

``` 
config system dhcp server
    delete <###PLACEHOLDER_DHCP-SERVER-ID###>  
end
```

### Stronger encryption for FortiGate and stronger Diffie-Hellman
Enabling strong encryption is recommended and a default setting.  
Using a higher value for DH leads to stronger encryption.  
**Note:** This is about HTTPS and SSH traffic towards the FortiGate. It has no impact on IKE/IPsec.

``` 
config system global
    set strong-crypto enable
    set dh-params 8192
end
``` 
**Note:** With `strong-crypto` enabled, various ciphers in `config system ssh-config` and its various settings disappear, due to being insecure. If you use a FortiGate to connect via SSH to devices that only support insecure options, you will not be able to connect to them.

### Disable static TLS keys for FortiGate-terminated traffic
Static, or non-ephemeral, keys can be considered weak and should be disabled for traffic terminated by a FortiGate.

``` 
config system global
    set ssl-static-key-ciphers disable
end
```

### USB auto-install of configurations and firmware should be disabled
By default, FortiGates allow the automatic installation of configuration files and upgrading of firmware if a USB stick is plugged in during boot. This can be used for low-touch provisioning, but in a production environment can lead to a device compromise.

``` 
config system auto-install
    set auto-install-config disable
    set auto-install-image disable
end
```

## Networking
### Configure SD-WAN
Employing SD-WAN has no drawbacks and makes future additions of WAN links easy. Additional SD-WAN features like SD-WAN rules and SLAs do not have to be used for the feature to be used.  
**Note:** Interfaces need to have certain references removed, like being in firewall policies, before they can be set as SD-WAN members.

``` 
config system sdwan
    set status enable
    config zone
        edit "<###PLACEHOLDER_SD-WAN-ZONE-NAME###>"  
        next
    end
    config members
        edit 0
            set interface "<###PLACEHOLDER_SD-WAN-MEMBER>"  
            set zone "<###PLACEHOLDER_SD-WAN-ZONE-NAME###>"  
        next
    end
end
```

### Put interfaces in zones
All interfaces that will be used in firewall policies, regardless of the type, should be put in zones, even if the zone would only have a single interface in it. Using zones makes interface replacements in firewall policies easy, and there are no drawbacks, except the maximum value of zones, which depends on the FortiGate model (see the [maximum values table](https://docs.fortinet.com/max-value-table)).

``` 
config system zone
    edit "<###PLACEHOLDER_ZONE-NAME###>"  
        set interface "<###PLACEHOLDER_ZONE-INTF###>"  
    next
end
```

### RFC1918 IP address space should be routed into a blackhole
**Attention:** This is one of the points that is important if the same device isn’t handling internal and external traffic.  
In a two-tier architecture with an internal and external firewall, you must not perform this on the internal firewall. The internal firewall, at a minimum, has connected routes and a default route towards the external firewall. By routing all RFC1918 space into a blackhole you will blackhole any RFC1918 traffic coming from the external firewall (VPN traffic in most cases) unless you have another form of routing between these two devices.  
Performing this on the external firewall in such a scenario should be fine in most cases, because you either route RFC1918 to the internal firewall or have distinct routes for the relevant networks.

Blackholing networks is a best practice for VPN specifically, since it helps when there is flapping on a VPN tunnel, because private traffic might get erroneously routed via the default route. Fortinet, with various VPN wizards, blackholes only the remote subnet, but blackholing the entire RFC1918 space saves on individual routes.

First three address objects for the RC1918 subnets get created, then a group for them all, and lastly a static blackhole route with a distance of 254 (a static route with a distance of 255 does not get installed). `allow-routing` needs to be enabled on all objects to be used in a static route.

``` 
config firewall address
    edit "<###PLACEHOLDER_RFC1918-10###>"  
        set allow-routing enable
        set subnet 10.0.0.0 255.0.0.0
    next
    edit "<###PLACEHOLDER_RFC1918-172###>"  
        set allow-routing enable
        set subnet 172.16.0.0 255.240.0.0
    next
    edit "<###PLACEHOLDER_RFC1918-192###>"  
        set allow-routing enable
        set subnet 192.168.0.0 255.255.0.0
    next
end
config firewall addrgrp
    edit "<###PLACEHOLDER_RFC1918###>"  
        set member "<###PLACEHOLDER_RFC1918-10###>" "<###PLACEHOLDER_RFC1918-172###>" "<###PLACEHOLDER_RFC1918-192###>"  
        set allow-routing enable
    next
end
config router static
    edit 0
        set dstaddr "<###PLACEHOLDER_RFC1918###>"  
        set distance 254
        set blackhole enable
    next
end
```

## Security profiles
### SSL/SSH profiles should block unsupported SSL/SSH communication
By default, SSL/SSH profiles allow traffic using unsupported SSL ciphers and negotiations, as well as bypass unsupported versions for SSH. These connections should be blocked.

``` 
config firewall ssl-ssh-profile
    edit "<###PLACEHOLDER_SSL-SSH-PROFILE###>"  
        config ssl
            set unsupported-ssl-cipher block
            set unsupported-ssl-negotiation block
        end
        config https
            set unsupported-ssl-cipher block
            set unsupported-ssl-negotiation block
        end
        config ftps
            set unsupported-ssl-cipher block
            set unsupported-ssl-negotiation block
        end
        config imaps
            set unsupported-ssl-cipher block
            set unsupported-ssl-negotiation block
        end
        config pop3s
            set unsupported-ssl-cipher block
            set unsupported-ssl-negotiation block
        end
        config smtps
            set unsupported-ssl-cipher block
            set unsupported-ssl-negotiation block
        end
        config ssh
            set unsupported-version block
        end
        config dot
            set unsupported-ssl-cipher block
            set unsupported-ssl-negotiation block
        end
    next
end
```

### Deep inspection SSL/SSH profiles should have a minimum SSL/TLS version of 1.2
The default value in deep inspectionSSL/SSH profiles of tls-1.1 is insecure and should be changed to tls-1.2
``` 
config firewall ssl-ssh-profile
    edit "<###PLACEHOLDER_SSL-SSH-PROFILE###>"  
        config https
            set status deep-inspection
            set min-allowed-ssl-version tls-1.2
        end
        config ftps
            set status deep-inspection
            set min-allowed-ssl-version tls-1.2
        end
    next
end
```

If `inspect-all deep-inspection` is set for SSL, the value can only be set for SSL.

``` 
config firewall ssl-ssh-profile
    edit "<###PLACEHOLDER_SSL-SSH-PROFILE###>"  
        config ssl
            set inspect-all deep-inspection
            set min-allowed-ssl-version tls-1.2
        end
    next
end
```

### Antivirus profiles should use the Outbreak Prevention database and quarantine files
If licensed for Antivirus, the Outbreak Prevention service uses third-party malware hashes to supplement FortiGuard-provided resources.  
The Outbreak Prevention service needs to be enabled per protocol. The GUI offers a simple way to enable this setting for most protocols, and in the CLI, it has to be enabled per protocol.  
Quarantine should be enabled if either FortiAnalyzer or a disk are available.

``` 
config antivirus quarantine
    set destination <###PLACEHOLDER_FortiAnalyzer/disk###>  
end
config antivirus profile
    edit "###PLACEHOLDER_AV-PROFILE###>"  
        config http
            set av-scan block
            set outbreak-prevention block
            set quarantine enable
        end
        config ftp
            set av-scan block
            set outbreak-prevention block
            set quarantine enable
        end
        config imap
            set av-scan block
            set outbreak-prevention block
            set quarantine enable
        end
        config pop3
            set av-scan block
            set outbreak-prevention block
            set quarantine enable
        end
        config smtp
            set av-scan block
            set outbreak-prevention block
            set quarantine enable
        end
        config nntp
            set av-scan block
            set outbreak-prevention block
            set quarantine enable
        end
        config cifs
            set av-scan block
            set outbreak-prevention block
            set quarantine enable
        end
    next
end
```

### Antivirus should use the extreme database if available
Select FortiGate models can use the extreme Antivirus database.  

**Note:** This can have a negative performance impact.

``` 
config antivirus settings
    set use-extreme-db enable
end
```

### Antivirus should use machine learning to better detect malware
Machine learning can augment the antivirus capabilities to better detect zero-day attacks.

``` 
config antivirus settings
    set machine-learning-detection enable
end
```

### Intrusion Prevention System profiles should block more signatures,  malicious traffic, and log packets
IPS profiles can block malicious URLs, botnet connections, and log packets.  
In addition, IPS profiles should override the default action for signatures that have at least a medium severity.

Three IPS profiles will be created:

1. Protect against client-side vulnerabilities
2. Protect against server-side vulnerabilities
3. Protect against client-and server-side vulnerabilities

**Note:** While these profiles might be too inclusive and have a negative performance impact, they work in most situations.

``` 
config ips sensor
    edit "<###PLACEHOLDER-IPS-CLIENT-PROFILE###>"  
        set block-malicious-url enable
        set scan-botnet-connections block
        config entries
            edit 0
                set location client
                set severity medium high critical
                set log-packet enable
                set action block
            next
            edit 0
                set location client
                set severity low
                set log-packet enable
            next
        end
    next
    edit "<###PLACEHOLDER-IPS-SERVER-PROFILE###>"  
        set block-malicious-url enable
        set scan-botnet-connections block
        config entries
            edit 0
                set location server
                set severity medium high critical
                set log-packet enable
                set action block
            next
            edit 0
                set location server
                set severity low
                set log-packet enable
            next
        end
    next
    edit "<###PLACEHOLDER-IPS-CLIENT-SERVER-PROFILE###>"  
        set block-malicious-url enable
        set scan-botnet-connections block
        config entries
            edit 0
                set location server client
                set severity medium high critical
                set log-packet enable
                set action block
            next
            edit 0
                set location server client
                set severity low
                set log-packet enable
            next
        end
    next
end
```

### DNS filter profiles should redirect Command & Control requests to the block portal
A DNS filter can be an additional layer to help prevent C&C communication.

``` 
config dnsfilter profile
    edit "<###PLACEHOLDER_DNS-PROFILE###>"  
        set block-botnet enable
    next
end
```

### DNS filters should log all queries and responses
In order to provide more visibility, DNS queries and responses can be logged.

``` 
config dnsfilter profile
    edit "<###PLACEHOLDER_DNS-PROFILE###>"  
        set log-all-domain enable
    next
end
```

### Application Control profiles should use Network Protocol Enforcement
Network Protocol Enforcement (NPE) is a feature that inspects traffic on specific ports and checks if the traffic matches the port. Attacks like DNS exfiltration can be recognized using this feature.  
There are only a select few protocols that can be used with NPE.

**Important points:**

* HTTP and HTTPS can be configured for NPE, but there are legitimate services and products (endpoint protection software like Trend Micro’s offerings, for example) that use these protocols to communicate with servers. For this reason, HTTP and HTTPS will not be included here, but you can enable these protocols if you want to test if it creates problems in your environment.  
* False positives can happen, but I have personally not experienced any issues, except with HTTP and HTTPS.

``` 
config application list
    edit "<###PLACEHOLDER_AC-PROFILE###>"  
        set control-default-network-services enable
        config default-network-services
            edit 0
                set port 25
                set services smtp
            next
            edit 0
                set port 22
                set services ssh
            next
            edit 0
                set port 53
                set services dns
            next
            edit 0
                set port 21
                set services ftp
            next
            edit 0
                set port 161
                set services snmp
            next
            edit 0
                set port 143
                set services imap
            next
            edit 0
                set port 110
                set services pop3
            next
        end
    next
end
```

### Application Control profiles should block applications communicating on non-default ports
Applications that use a well-known port for other communication can be an indicator of malicious traffic.

**Attention:** There are legitimate applications that work like this, so test this setting thoroughly.

``` 
config application list
    edit "<###PLACEHOLDER_AC-PROFILE###>"  
        set enforce-default-app-port enable
    next
end
```

### Web filter profiles should block invalid URLs and rate URLs by domain and IP address
Blocking malicious URLs is a simple matter, and rating servers by IP can cover blind spots.

``` 
config webfilter profile
    edit "<###PLACEHOLDER_WF-PROFILE###>"  
        set options block-invalid-url
        config ftgd-wf
            set options rate-server-ip
        end
    next
end
```

### Web filter profiles should log more information
Web filters can log all URLs and provide extended logging information.

``` 
config webfilter profile
    edit "<###PLACEHOLDER_WF-PROFILE###>"  
        set log-all-url enable
        set extended-log enable
    next
end
```

### Enable and use FortiGate Cloud Sandbox
FortiGates that have an active Antivirus license have access to an instance of a cloud-hosted sandbox, which can be used in antivirus and web filter profiles.

The steps for this are:

1. Enable the option in the GUI (optional)  
   1. To see this option in the GUI, refresh your browser, or log out and back in, to see it in Security Fabric -> Fabric Connectors -> Sandbox.  
2. Select a region
3. After some time, the **Version** and **Signature Count** in the GUI should update.  
4. Enable the feature in antivirus and web filter profiles.

``` 
config system global
    set gui-fortigate-cloud-sandbox enable
end

execute forticloud-sandbox region
0  Europe
1  Global
2  US
3  Japan
Please select cloud sandbox region[0-3]:0

config antivirus profile
    edit "<###PLACEHOLDER_AV-PROFILE###>"  
        set analytics-db enable
        config http
            set fortisandbox monitor
        end
        config ftp
            set fortisandbox monitor
        end
        config imap
            set fortisandbox monitor
        end
        config pop3
            set fortisandbox monitor
        end
        config smtp
            set fortisandbox monitor
        end
    next
end
config webfilter profile
    edit "<###PLACEHOLDER_WF-PROFILE###>"  
        config web
            set blocklist enable
        end
    next
end
```
