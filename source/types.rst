################################
Dell EMC Networking Puppet types
################################

The Dell EMC Networking Puppet types facilitate device provisioning running Dell EMC Networking OS10 software. This information describes the Puppet types and attributes available in the Dell EMC Networking Puppet module.

Type: os10_route
################

The ``os10_route`` resource type is used to manage static routes in OS10 switches.

**Attributes**

==============      ================================================
Attribute           Description 
==============      ================================================
destination         Target IP address to which the route must be configured
prefix_len          Netmask of the target IP address
next_hop_list       List of the next-hop IP address for the route to be configured
ensure              Determine whether the route entry should be present or not
==============      ================================================

Type: os10_snmp
###############

The ``os10_snmp`` resource type is to used to manage SNMP configurations in OS10 Enterprise Edition switches. The os10_snmp resource is not an ensurable type and does not have an ensure attribute.

**Attributes**

=====================           ================================================
Attribute                       Description
=====================           ================================================
``community_strings``           This property is a dictionary of community string with its access right; will be the only list of community string entries present in the SNMP configuration (for example, ``{'public'=>'ro', 'private'=>'rw'}``
``contact``                     Contact property of SNMP server; there can be only one entry for contact, and an empty string for contact will remove the contact entry from the SNMP configuration
``location``                    Location property of the SNMP server; there can be only one entry for location, and an empty string for location will remove the location entry
``enabled_traps``               Dictionary of entries where the key is trap category and values are the list of subcategory or all to enable traps for all subcategory items
``trap_destination``            Dictionary of entries where the key is list of [ip,Port] and value is a list with version string ("v1"/"v2") and community string
=====================           ================================================

Type: os10_monitor
##################

The ``os10_monitor`` resource type is to used to manage port-monitoring (mirroring) session configuration in OS10 Enterprise Edition switches.

**Attributes**

===============  ===============================================================
Attribute        Description
===============  ===============================================================
``id``           ID of the monitor session in the switch; ID needs to be unique (1 and 18)
``source``       Values of the interfaces that will be configured as source interfaces for this monitoring session (for example, ['ethernet 1/1/9', 'ethernet 1/1/10'])
``destination``  Name of the destination interface to which traffic has to be mirrored (for example, 'ethernet 1/1/10')
``flow_based``   Value specifying whether to enable or disable flow-based monitoring (optional value which defaults to false)
``shutdown``     Property will decide whether to enable or disable the monitoring session; if set to false, the session will be configured but in shutdown state (optional value which defaults to true)
===============  ===============================================================

Type: os10_interface
####################

The ``os10_interface`` resource type is used to manage interface configuration in OS10 Enterprise Edition switches.

**Attributes**

===================   ====================================================================
Attribute             Description
===================   ====================================================================
``desc``              Description of the interface
``mtu``               Maximum transmission unit of the interface
``switchport_mode``   Switchport mode of the interface; can be either trunk or access in the case of switchport, or can be false when not in L2 mode (trunk, access, absent)
``admin``             Administrative state of the interface (up, down)
``ip_address``        IPv4 address and mask of the interface in ip/prefixlen format
``ipv6_address``      IPv6 address and mask of the interface in ip/prefixlen format
``ipv6_autoconfig``   Enable or disable IPv6 autoconfig (true, false)
``ip_helper``         List of IP address for the interface to which UDP broadcasts need to be forwarded to
===================   ====================================================================

Type: os10_image_upgrade
########################

The ``os10_image_upgrade`` resource type is used to upgrade/downgrade OS10 Enterprise Edition images by providing the filename and location of the image.

**Attribute**

=============   ===============================================================
Attribute       Description                                         
=============   ===============================================================
``image_url``   Location of the binary image in the remote server; image will be downloaded and installed in the standby partition of the switch
=============   ===============================================================

Type: os10_bgp
##############

The resource definition for ``os10_bgp`` that is used to configure base BGP configuration in OS10 Enterprise Edition switches.

**Attributes**

====================================  ====================================================================
Attribute                             Description                                         
====================================  ====================================================================
``ensure``                            Determines whether the BGP configuration should be present or not (true, false)
``asn``                               Autonomous system (AS) number of the BGP configuration (1 to 4294967295 or 0.1 to 65535.65535)
``router_id``                         Configures the IP address of the local BGP router instance
``max_path_ebgp``                     Configures the maximum number of paths to forward packets through eBGP (1 to 64)
``max_path_ibgp``                     Configures the maximum number of paths to forward packets through iBGP (1 to 64)
``graceful_restart``                  Configures graceful restart capability (true, false)
``log_neighbor_changes``              Configures logging of neighbors up/down
``fast_external_fallover``            Configures reset session if a link to a directly connected external peer goes down
``always_compare_med``                Configures comparing MED from different neighbors
``default_loc_pref``                  Configures the default local preference value (1 to 4294967295)
``confederation_identifier``          Sets the autonomous system identifier for the confederation routing domain (1 to 4294967295 or 0.1 to 65535.65535)
``confederation_peers``               Configures peer AS number entries in BGP confederation as a list (1 to 4294967295 and 0.1 to 65535.65535)
``route_reflector_client_to_client``  Configures client-to-client route reflection
``route_reflector_cluster_id``        Configures route-reflector cluster-id (1 to 4294967295 or A.B.C.D IPv4 address format)
``bestpath_as_path``                  Configures the best-path selection to either ignore or include prefixes received from different AS paths during multipath calculation
``bestpath_med_confed``               Configures best-path to compare MED among confederation paths
``bestpath_med_missing_as_worst``     Configures best-path to treat missing MED as the least preferred one
``bestpath_routerid_ignore``          Configures best-path computation to ignore router identifier
====================================  ====================================================================

Type: os10_bgp_af
#################

**Attributes**

==========================   ===================================================
Attribute                    Description                                         
==========================   ===================================================
``ensure``                   Configures whether the BGP address family section should be present or not
``require``                  Configures the dependant os10_bgp configuration that should be configured before applying the os10_bgp_af configuration
``asn``                      AS number of the BGP configuration (1 to 4294967295 or 0.1 or 65535.65535)
``ip_ver``                   Configures the IP version of this instance of address family configuration (ipv4, ipv6)
``aggregate_address``        Configures ipv4/ipv6 BGP aggregate address and mask; values should be of the same version as provided in ``ip_ver`` parameter
``dampening_state``          Enable or disable route-flap dampening; when dampening_state is set to true, all timers should be defined
``dampening_half_life``      Sets dampening half-life time for the penalty (1 to 45)
``dampening_reuse``          Sets the time value to start reusing a route (1 to 20000)
``dampening_suppress``       Sets the time value to start suppressing a route (1 to 20000)
``dampening_max_suppress``   Sets the maximum time duration to suppress a stable route (1 to 255)
``dampening_route_map``      Configures the name of route-map to specify criteria for dampening (up to 140 characters)
``default_metric``           Sets the default metric of redistributed routes (1 to 4294967295)
``network``                  List of IPs and mask along with optional route-map string
``redistribute``             Configures routing protocols that need to be redistributed; valid value is a list of (protocol value); protocol can be connected, ospf, or static; value can be blank or route-map string in the case of connected, static and blank or process-id in the case of ospf
==========================   ===================================================

## Type: os10_bgp_neighbor

**Attributes**

============================   =================================================
Attribute                      Description                                         
============================   =================================================
``require``                    Configures the dependant os10_bgp configuration that should be configured before applying the os10_bgp_neighbor configuration
``ensure``                     Configures whether the os10_bgp_neighbor should be present or not
``asn``                        AS number of the BGP configuration (1 to 4294967295 or 0.1 to 65535.65535)
``neighbor``                   Specifies a neighbor router IP address or template name for the given configuration (IPv4 or IPv6 address; up to 16 characters)
``type``                       Specifies whether the configuration is for neighbor IP or template
``advertisement_interval``     Configures the minimum interval between sending BGP routing updates
``advertisement_start``        Configures the delay initiating OPEN message for the specified time
``connection_retry_timer``     Configures the peer connection retry timer
``remote_as``                  Specifies the AS number of the BGP neighbor
``remove_private_as``          Enables or disables configuration to remove private AS number from outbound updates
``shutdown``                   Sets the shutdown state of the neighbor
``password``                   Sets the MD5 password for authentication (up to 128 characters)
``send_community_standard``    Enables or disables sending standard community attribute
``send_community_extended``    Enables or disables sending extended community attribute
``peergroup``                  Configures the neighbor to BGP peer-group; inherit configuration of peer-group template; template should be an existing configuration
``ebgp_multihop``              Configures the maximum-hop count value allowed in eBGP neighbors that are not directly connected (1 to 255)
``fall_over``                  Configures the session fall on peer-route loss
``local_as``                   Configure the local AS number for the BGP peer
``route_reflector_client``     Configures a BGP neighbor as route-reflector client
``weight``                     Configure the default weight for routes from the neighbor interface (1 to 4294967295)
============================   =================================================

## Type: os10_bgp_neighbor_af

The resource definition for os10_bgp_neighbor_af that is used to configure address family subconfiguration (for both IPv4 and IPv6) under BGP neighbor subconfiguration.

**Attributes**

====================   =========================================================
Attribute              Description
====================   =========================================================
``require``            Configures the dependant os10_bgp configuration that should be configured before applying the os10_bgp_neighbor configuration
``ensure``             Configures whether the `bgp_neighbor_af` subconfiguration should be present or not
``asn``                AS number of the BGP configuration (1 to 4294967295 or 0.1 to 65535.65535)
``neighbor``           Configures the neighbor route IP address to which the current address family subconfiguration
``type``               Specifies whether the neighbor configuration is of type ip or template
``ip_ver``             Configures either ipv4 or ipv6 address family
``activate``           Enables the address family for this neighbor
``allowas_in``         Configures the allowed local AS number in as-path (1 to 10)
``add_path``           Configures the setting to send or receive multiple paths; blank string removes the configuration
``distribute_list``    Filters networks in routing updates; valid parameter is an array of two prefix-list names (up to 140 characters) for applying policy to incoming and outgoing routes respectively
``next_hop_self``      Enables or disables the next-hop calculation for this neighbor
``route_map``          Configures the names of the route-map; valid parameter is an array of two route-map names (up to 140 characters) for filtering incoming and outgoing routing updates
====================   =========================================================

## Type: os10_lldp

The ``os10_lldp`` resource type is to used to manage global LLDP configuration in OS10 Enterprise Edition switches. The os10_lldp resource is not an ensurable type and hence does not have an ensure attribute.

**Attributes**

===============================   =============================================
Attribute                         Description
===============================   =============================================
``holdtime_multiplier``           Configures the holdtime multiplier (2 to 10); empty string will remove the holdtime multiplier value from the LLDP configuration
``reinit``                        Configures the reinit value (1 to 10); empty string will remove the reinit value from the LLDP configuration
``timer``                         Configures the timer value ((5 to 254); empty string will remove the timer value from the LLDP configuration
``med_fast_start_repeat_count``   Configures the med fast start repeat count value (1 to 10); empty string will remove the med fast start repeat count value from the LLDP configuration
``enable``                        Enables disables LLDP globally
``med_network_policy``            Specifies the hash entries with a set of hash keys id<1-32>, app<guest-voice, guestvoice-signaling, softphone-voice, streaming-video, video-conferencing, voice-signaling, voice, video-signaling>, vlan_id<1-4093>, vlan_type<tag/untag>, priority<0-7>, dscp<0-63>
===============================   =============================================

## Type: os10_lldp_interface

The ``os10_lldp_interface`` resource type is to used to manage LLDP configuration per interface in OS10 Enterprise Edition switches. The os10_lldp resource is not an ensurable type and does not have an ensure attribute. The per-interface name is given as argument for the resource.

**Attributes**

==================================   ==========================================
Attribute                            Description
==================================   ==========================================
``receive``                          Enables or disables the reception of LLDP for that interface (true, false)
``transmit``                         Enable or diables the transmission of LLDP for that interface (true, false)
``med``                              Enables or disables the MED LLDP for that interface; LLDP MED can be enabled only when LLDP transmit and receive are enabled; LLDP receive/transmit can be disabled only when LLDP MED is disabled (true, false)
``med_tlv_select_inventory``         Enables or disables the MED TLV select inventory LLDP for that interface (true, false)
``med_tlv_select_network_policy``    Enables or disables the MED TLV select network policy LLDP for that interface (true, false)
``med_network_policy``               Specifies MED policy IDs with a range of <1-32> to add and remove network policies
``tlv_select``                       Specifies the hash of key value pair with LLDP TLV select option as key and suboption as array of values; tlv-select for all the interfaces are enabled by default in the device; values provided in the parameter are to disable the options per interface; values not in the list will be enabled; values for tlv_select options and suboptions are basic-tlv => ["management-address", "port-description", "system-capabilities", "system-description", "system-name"], dcbxp => [""], dcbxp-appln => ["iscsi"], dot3tlv => ["macphy-config", "max-framesize"], dot1tlv => ["link-aggregation", "port-vlan-id"]
==================================   ==========================================
