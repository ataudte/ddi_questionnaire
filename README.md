
# Motivation

This document serves as an inspiration for a DNS, DHCP, and IPAM (DDI) evaluation. It begins with general aspects of the solution, followed by a questionnaire on DNS, DHCP, and IPAM. Drawings and sketches should be included where available.

# Table of Contents
<details>
  <summary>Table of Contents</summary>
  
- [Weighting of Requirements](#weighting-of-requirements)
- [Overall Requirements](#overall-requirements)
  - [Requirements for Accessibility](#requirements-for-accessibility)
  - [Requirements for Reliability](#requirements-for-reliability)
  - [Requirements for Architecture](#requirements-for-architecture)
  - [Requirements for Maintenance](#requirements-for-maintenance)
- [Component Requirements](#component-requirements)
  - [Requirements for Hardware Appliances](#requirements-for-hardware-appliances)
  - [Requirements for Management Database](#requirements-for-management-database)
  - [Requirements for User Management](#requirements-for-user-management)
  - [Requirements for Migration Tool](#requirements-for-migration-tool)
  - [Requirements for Application Programming Interface](#requirements-for-application-programming-interface)
  - [Requirements for Discovery](#requirements-for-discovery)
  - [Requirements for Reporting](#requirements-for-reporting)
  - [Requirements for Documentation and Support](#requirements-for-documentation-and-support)
- [Requirements for DNS Platform](#requirements-for-dns-platform)
  - [Requirements for DNS Zone Management](#requirements-for-dns-zone-management)
  - [Requirements for DNS Record Management](#requirements-for-dns-record-management)
  - [Requirements for further DNS Capabilities](#requirements-for-further-dns-capabilities)
  - [Requirements for DNSSEC Management](#requirements-for-dnssec-management)
- [Requirements for DHCP Platform](#requirements-for-dhcp-platform)
  - [Requirements for DHCP Scope Management](#requirements-for-dhcp-scope-management)
  - [Requirements for DHCP Options](#requirements-for-dhcp-options)
  - [Requirements for DHCP Templates](#requirements-for-dhcp-templates)
  - [Requirements for further DHCP Capabilities](#requirements-for-further-dhcp-capabilities)
- [Requirements for Address Space Management](#requirements-for-address-space-management)
  - [Requirements for Address Space Access Rights](#requirements-for-address-space-access-rights)
  - [Requirements for Meta Data](#requirements-for-meta-data)
  - [Requirements for further Address Space Capabilities](#requirements-for-further-address-space-capabilities)
  - [Requirements for IPv6](#requirements-for-ipv6)
- [Appendix](#appendix)
  - [Footnotes](#footnotes)
</details>

# Weighting of Requirements

- must-have (critical to meet the requirements)
- should-have (important but not necessary to meet the requirements)
- could-have (desirable but not necessary to meet the requirements)

# Overall Requirements

## Requirements for Accessibility

- management provides a web-based user interface (no fat client required)
- management web UI allows deployment of custom SSL certificate
- web UI supports standard browsers (Microsoft Edge, Firefox, Chrome, etc.)
- web UI prevents invalid inputs (IPs, MACs, FQDNs, etc.)
- web UI is compliant with at least WCAG[^1] 2.1 Level AA guidelines
- web UI supports assistive technologies (screen readers, voice control tools, etc.)
- web UI supports keyboard navigation and skip-to-content features
- web UI uses accessible color contrasts and scalable fonts
- web UI provides tree-view for hierarchical objects (domain structure, address space, etc.)
- tree-view has no limitation regarding the number of hierarchy levels
- web UI provides global & user-based data restore
- deleting an entity in web UI removes linked entities (network > IPs, IP > A/PTR, etc.)
- web UI provides global search functionality
- multiple search properties can be combined
- complex search properties can be saved for later re-use

## Requirements for Reliability

- communication between all components must be encrypted
- passwords aren't stored in clear-text (config. files, database, etc.)
- no limitations in terms of special characters used within passwords
- system level access (root access) is available for all components
- system level access is not required for job routine operations
- failure of central management must not affect DNS and DHCP services
- updates from DNS and DHCP servers (DDNS, leases) queued till management available
- all components provide an integration with external logging (syslog, SIEM[^2], etc.)
- high reliability of the core services DNS and DHCP
- automated restart in case service is down

## Requirements for Architecture

- possible separation of services (DNS and DHCP)
- options for appliance-based or container-based solution
- management of 3rd party DNS and DHCP services (Microsoft, Linux, cloud, etc.)
- central management of all DNS and DHCP data and services
- IPAM instance responsible for creation of DNS and DHCP configuration files
- IPAM instance supports multi-tenancy (overlapping address and name space)
- conflicts between tenants can be reviewed in web UI
- selected duplicated objects allowed in IPAM instance (hostnames, addresses, etc.)
- IPAM instance allows enabling and disabling objects (hostnames, reservations, etc.)
- IPAM instance can lock objects for other user accounts (prevents parallel administration)

## Requirements for Maintenance

- central management allows consolidated patch and update management
- central management provides backward compatibility (version of IPAM & service)
- patch or update of IPAM instance without the need to patch or update service instances
- built-in roll-back mechanism for all patches and updates
- web UI provides scheduled tasks for daily routine operations (add, modify, delete objects)
- debug mode can be activated easily (e.g. within the web UI)
- solution must monitor itself
- monitoring of general OS resources (CPU, RAM, Disk, etc.)
- monitoring and probing of DNS and DHCP services

# Component Requirements

## Requirements for Hardware Appliances

- platform comes with fail-safe components (power, disks, interfaces, etc.)
- platform provides physical redundancy for IPv4 and IPv6 (clustering)
- platform of the IPAM instance comes with next business day hardware refresh
- service instance comes with next business day hardware refresh
- local settings of service instace is stored in database for easy hardware replacement
- all components provide a local firewall
- only required services are running on all components (e.g. no web UI on service instances)
- service instance provides dedicated management interface over IPv4 and IPv6 (separate management and service provisioning)
- platform manufacturer provides KYHD[^3] service

## Requirements for Management Database

- IPAM instance has no technical limitations (number of managed DNS/DHCP objects)
- IPAM instance provides system level access (root access)
- IPAM instance provides redundancy option for its database
- IPAM instance comes with built-in backup & restore capabilities
- backups can be initiated manually or time-controlled
- backup & restore process works with external storage
- database allows tenant-specific restore

## Requirements for User Management

- access rights in the IPAM instance are based on users, groups & roles
- groups & roles can be nested and combined
- IPAM instance provides external authentication (AD[^4], LDAP, Radius, TACACS+, etc.)
- web UI provides single-sign-on (SSO) and multi-factor-authentication (MFA)
- all components provide external authentication for command line accounts (Unix-Shell)
- IPAM instance supports multiple authenticators (fallback to secondary authenticator)
- web UI provides centralized session & change tracking (who, when, what)
- web UI provides object-based session & change tracking (who, when, what)
- session & change tracking displays previous and new value of changed property
- tracking history allows to search for changes (e.g. by account or time frame)
- time frame of changes to keep in tracking history can be controlled (automated deletion)
- tracking history can be forwarded to party systems (syslog, SIEM[^2], Splunk, etc.)
- access rights for tracking history can be controlled (who can see what in history)
- groups or roles in web UI can be mapped against external authentication (e.g. AD[^4] groups)
- access to menus & actions in the web UI can be controlled based on permissions
- IPAM instance supports the import of access rights
- web UI provides an overview of assigned access rights per user, group or role

## Requirements for Migration Tool

- manufacturer provides migration tool for analysis & design
- migration tool provides optimization feature (multi-homed records, orphaned records)
- IPAM instance supports the import of DNS and DHCP data
  - import of DNS zones, records & options
  - import of DHCPv4 scopes, reservations, leases & options
  - import of DHCPv6 scopes, reservations, leases & options
  - import of IPv4 networks & network ranges
  - import of IPv6 networks & network ranges
  - import of meta data
  - error handling (syntax issues, etc.)
- IPAM instance supports the import of data from previous versions into newer versions

## Requirements for Application Programming Interface

- web UI provides a dashboard for daily routine (add host, delete device, etc.)
- IPAM instance provides pre-defined command line interface (CLI[^5])
- IPAM instance comes with web service API (REST[^6], SOAP[^7], etc.)
- IPAM instances provides custom UI to hide features & functions

## Requirements for Discovery

- IPAM instance provides ping sweep (ICMP[^8] echo request & reply)
- IPAM instance and service instance provide SNMP discovery capability (v1, v2c, v3)
- multiple SNMP community strings can be provided
- discovery supports of link layer discovery protocol (LLDP)
- discovery support of Cisco discovery protocol (CDP)
- discovery results include IP, MAC, connected switch, connected router & time
- discovery takes name resolution into account (PTR Records)
- discovery provides port scan (e.g. nmap)
- VMware infrastructure can be discovered
- cloud-based infrastructure can be discovered
- IPAM instance lists differences between discovery results & the database
- IPAM instance provides manual & automatic import of discovery results

## Requirements for Reporting

- all transactions are logged centrally (user access, API calls, system changes, etc.)
- IPAM instance provides report templates (utilization, unused objects, etc.)
- reporting supports various formats (CSV, HTML, PDF, XLS, etc.)
- reporting can be initiated manually or time-controlled (emailing, upload, etc.)
- enhanced reporting through read-only direct database access is supported
- lists displayed in web UI can be exported (including search results & tracking history)
- reporting through a secondary or dedicated database is supported

## Requirements for Documentation and Support

- manufacturer provides quick start guides (PDF, e-learning, videos, etc.)
- manufacturer provides offline admin guides (PDF)
- manufacturer provides API guides (PDF)
- manufacturer provides API examples (Perl, Python, etc.)
- manufacturer operates knowledge base (known issues, best practices, etc.)
- manufacturer operates ticketing system (issue tracking system)
- web UI provides context-sensitive or mouse-over help

# Requirements for DNS Platform

- IPAM instance can manage cloud-based DNS services (Azure DNS, Route 53, etc.)
- service instance provides high available DNS (cluster, anycast, HA DNS, etc.)
- IPAM instance and service instance can handle split DNS & DNS views
- service instance supports recursive DNS
- service instance supports restrictions (access lists, signatures, etc.)
- IPAM instance provides access rights for all DNS objects (hide, view, add, change, delete)

## Requirements for DNS Zone Management

- IPAM instance supports management for all DNS objects
  - management of authoritative forward zones
  - management of authoritative reverse zones
  - management of DNS updates (DDNS)
  - management of zone transfers (AXFR & IXFR)
  - management of ACLs for dynamic DNS & zone transfers
  - management of forwarding and stub zones (selective/conditional     forwarding)
  - management of delegations within the environment
  - management of delegations to non-managed party server
  - management of hidden primary DNS server
  - management of hidden secondary DNS server
  - management of secondary with external non-managed primary
- web UI support management of international domain names (IDN)
- web UI allows to move zones including child zones & records
- web UI allows to move zones from existing DNS server to another
- web UI supports to rename zones including dependencies (delegations, etc.)
- web UI supports to change zone type (primary zone $\Rightarrow$ hidden primary zone, etc.)
- web UI offers grouping for DNS servers\ (assignment of server group to zone instead of individual servers)
- IPAM instance and service instance support multi-primary DNS

## Requirements for DNS Record Management

- dependant records are linked in IPAM instance for data consistency (CNAME, SRV, A/PTR, etc.)
  - linked and dependant entities of a record can be accessed easily
  - movement of record takes dependant records into account
  - renaming of record takes dependant records into account
- IPAM instance supports the management of reverse-only records (PTR without A)
- web UI lists records of secondary zones of non-managed primary
- IPAM instance and service instance support the integration with Active Directory (GSS-TSIG signed dynamic DNS updates)
- IPAM instance and service instance support RFC-compliant DNS records
  - RFC 1035 records (A, CNAME, PTR, etc.)
  - RFC 2782 records (SRV)
  - RFC 3596 records (AAAA)
- web UI provides real-time visibility for dynamic DNS records
- web UI validates user input for zones, records & options

## Requirements for further DNS Capabilities

- IPAM instance manages of DNS-based authentication of named entities (DANE)
- web UI provides naming convention for records
- web UI provides naming restriction for records
- web UI supports management of DNS options at server, view and zone level
- web UI provides visibility for inheritance of DNS options
- web UI provides templates for zones and records (e.g. www, mail, etc.)
- changes to a template can be re-applied with linked objects
- IPAM instance and service instance support response policy zones (RPZ)
- web UI supports management of DNS options missing in UI (extensions)
- web UI provides access to name server control utility (RNDC) features (flush DNS, etc.)
- web UI provides access DNS logs
- web UI manages sub domains without the need for subzones (dotted hostnames)
- web UI provides access rights for sub domains of dotted hostnames
- web UI supports the `$GENERATE` zone statement
- web UI provides visibility to differentiate dynamic updates (DDNS) from static records
- web UI allows on-the-fly DNS changes through the user (by using dynamic DNS)
- time to live (TTL) values can be managed for each DNS record individually
- web UI allows access to DNS statistics (op codes, query types, etc.)
- web UI supports bulk changes for DNS records (e.g. renaming based on patterns)

## Requirements for DNSSEC Management

- hosting provider supports DNSSEC-enabled zones
  - incl. record types of RFC 4034 (DNSKEY, RRSIG, NSEC, DS), RFC 5155 (NSEC3), RFC 7671 (TLSA) & RFC 8078 (CDS/CDNSKEY)
- hosting provider can enable DNSSEC signing per zone
- IPAM instance can enable DNSSEC validation per server
- validation of non-managed signed zones can be disabled (negative trust anchors)
- hosting provider support NSEC and NSEC3 with various algorithms
- hosting provider allows central key storage and management
- assignment of parameters for zone signing is managed centrally via policies
- hosting provider supports automatic key roll-over for ZSK
- hosting provider supports automatic key roll-over for KSK
- hosting provider supports emergency key roll-over for ZSK
- hosting provider supports emergency key roll-over for KSK
- hosting provider supports key export for ZSK
- hosting provider supports key export for KSK
- hosting provider sends notifications for automatic ZSK roll-overs
- hosting provider sends notifications for KSK expiration
- hosting provider supports ZSK roll-over with double signatures
- hosting provider supports KSK roll-over with double signatures
- hosting provider supports ZSK roll-over with pre-publish signatures
- hosting provider supports KSK roll-over with pre-publish signatures

# Requirements for DHCP Platform

- service instance provides high available DHCP (cluster, DHCP failover, etc.)
- IPAM instance provides access rights for all DHCP objects (hide, view, add, change, delete)
- web UI provides access DHCP logs

## Requirements for DHCP Scope Management

- IPAM instance supports management of DHCPv4 & DHCPv6 scopes
- IPAM instance supports splitting & merging DHCPv4 & DHCPv6 scopes
- IPAM instance supports moving scopes from existing DHCP server to another
- IPAM instance supports management of DHCPv4 & DHCPv6 reservations (DUID[^9])
- web UI provides real-time visibility for DHCP leases
- lease can be converted into Reservation in the web UI
- web UI provides DHCPv4 & DHCPv6 lease history
- existing leases can be deleted and freed in the web UI
- expiration or deletion of lease triggers removal of associated entities (dynamic DNS)
- service instance supports generation of hostnames for clients not sending a name
- leases can be assigned based on MAC address or client ID (dhcp-client-identifier)
- service instance supports one lease per client
- ping-before-assign functionality can be managed in web UI
- service instance supports legacy implementations (BOOTP, etc.)

## Requirements for DHCP Options

- IPAM instance supports management of DHCP match classes
- IPAM instance supports management of DHCPv4 options (RFC 2132)
- IPAM instance supports management of DHCPv6 options (RFC 3319)
- web UI supports management of DHCP options missing in UI (extensions)
- certain DHCPv4 option value can be changed in web UI wherever used in the system
- certain DHCPv6 option value can be changed in web UI wherever used in the system
- web UI manages of DHCP options at server, network, pool & reservation level
- web UI provides visibility for inheritance of DHCP options
- IPAM instance supports management of custom options (128  - 254)

## Requirements for DHCP Templates

- web UI provides templates for IPv4 & IPv6 networks
- web UI provides templates for IPv4 & IPv6 scopes
- web UI provides templates for IPv4 & IPv6 reservations
- changes to a template can be re-applied with linked objects

## Requirements for further DHCP Capabilities

- IPAM instance provides utilization altering for DHCP scopes (UI, mails, SNMP[^10], etc.)
- web UI provides centralized management of MAC addresses & MAC pools
- web UI provides visibility into DHCP fingerprinting details
- web UI allows access to DHCP statistics (leases per second, etc.)
- web UI provides monitoring & alarming of DHCP thresholds
- web UI supports bulk changes for DHCP reservations and & scopes

# Requirements for Address Space Management

- web UI allows management of private IPv4 address space (RFC 1918)
- web UI allows management of public IPv4 address space
- web UI allows management of multicast IPv4 address space (224.0.0.0/4)
- web UI support the allocation of next available IPv4 address range
- web UI support the allocation of next available IPv4 address network
- web UI support the allocation of next available IPv4 address
- web UI support the allocation of next available IPv4 address from offset
- web UI support the allocation of next available IPv4 address within specific range
- web UI support the management of unique local address (ULA) ranges (fc00::/7)
- web UI support the management of global unicast address (GUA) ranges (2000::/3)
- web UI support the management of multicast IPv6 ranges (ff00::/8)
- web UI prevents or warns about reserved address ranges (TEREDO, 6to4, etc.)
- link-local addresses (FE80::/10) can be documented in web UI
- web UI is able to restrict allowed subnet sizes (e.g. /24 for IPv4, /64 for IPv6)
- web UI support the allocation of next available IPv6 address range
- web UI support the allocation of next available IPv6 address network
- web UI support the allocation of next available IPv6 address
- web UI support the allocation of next available IPv6 address from offset
- web UI support the allocation of next available IPv6 address within specific range
- web UI support the allocation of overlapping address spaces

## Requirements for Address Space Access Rights

- IPAM instance provides access rights for all IPAM objects (hide, view, add, change, delete)
- IPAM instance provides access rights for non-allocated addresses & address ranges
- user's access rights are taken into account when searching in the web UI

## Requirements for Meta Data

- IPAM instance provides unlimited meta data (number of field per object)
- IPAM instance provides meta data for all object types (DNS, DHCP, users, etc.)
- web UI supports various values for meta data (text, integer, mail, boolean, etc.)
- meta data fields can be mandatory in the web UI
- predefined values create drop-down menus for meta data in the web UI
- drop-down menus of meta data fields can be nested
- web UI prevents invalid inputs for meta data
- web UI provides report of unused meta data definitions
- web UI provides non-DNS/DHCP-related structures (geographical, organizational, etc.)
- objects in the web UI can be tagged or grouped

## Requirements for further Address Space Capabilities

- management of DNS, DHCP & IPAM objects in the web UI is context-independent
- web UI provides a consolidated view of DNS & DHCP objects in IPAM networks
- available DNS zones per subnet can be restricted in the web UI
- networks can be structured into ranges and child ranges in the web UI
- web UI provides structuring tools (split, merge, move, resize, etc.)
- web UI allows the management of DNS & DHCP options at IPAM level
- IP addresses or ranges can be reserved for future use in the web UI
- objects can be cloned in the web UI (IPs, subnets, zones, etc.)
- dependencies of entities are linked for data consistency (device, reservation, record, etc.)
- linked entities can be accessed easily in the web UI
- networks can be moved incl. all IPs and dependencies in the web UI
- IP addresses can be moved incl. all dependencies in the web UI
- associations of AD sites can be managed in the web UI
- MAC addresses for static non-DHCP IPv4 addresses can be documented in the web UI
- web UI provides monitoring & alarming of network utilization
- web UI supports bulk changes for IP addresses, networks and ranges

## Requirements for IPv6

- web UI is accessible via IPv6 (dual-stack, native)
- API is accessible via IPv6 (dual-stack, native)
- service instance provides DNS via IPv6
- IPAM instance manages DNS via IPv6
- service instance provides DHCP via IPv6 (stateful and stateless)
- IPAM instance manages DHCP via IPv6
- web UI displays DUID[^9] and IAID[^11] for DHCPv6 clients
- web UI support management of DHCPv6 redundancy (RFC 6853)
- service instances can form an IPv6 cluster
- web UI allows central management of company's IPv6 prefix (given GUA[^12] of company)
- web UI supports management of dual-stack devices (multi-homed)
- web UI provides navigation between IPv4 & IPv6 information of dual-stack devices
- IPv6 networks can be linked with corresponding IPv4 networks in the web UI (1:1, 1:n & n:n associations)
- web UI provides navigation between linked IPv4 & IPv6 networks

# Appendix

## Footnotes

[^1]: Web Content Accessibility Guidelines
[^2]: Security Information & Event Management
[^3]: Keep Your Hard Drive
[^4]: Active Directory
[^5]: Command-Line Interface
[^6]: Representational State Transfer Application Programming Interface
[^7]: Simple Object Access Protocol
[^8]: Internet Control Message Protocol
[^9]: DHCP Unique Identifier
[^10]: Simple Network Management 
[^11]: Identity Association Identifier
[^12]: Global Unicast Address