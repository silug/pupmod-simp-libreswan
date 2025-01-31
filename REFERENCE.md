# Reference

<!-- DO NOT EDIT: This document was generated by Puppet Strings -->

## Table of Contents

### Classes

* [`libreswan`](#libreswan): Installs and configures libreswan to provide IPSEC capabilities.
* [`libreswan::config`](#libreswan--config): Configures `ipsec.conf` and necessary directories.
* [`libreswan::config::firewall`](#libreswan--config--firewall): Ensures that the required firewall rules are defined
* [`libreswan::config::pki`](#libreswan--config--pki): Ensure that the `simp/pki` PKI certificates are loaded into the IPSEC NSS Database.
* [`libreswan::config::pki::nsspki`](#libreswan--config--pki--nsspki): Ensure that the PKI certificates are loaded into the NSS Database used by the IPSEC process.
* [`libreswan::install`](#libreswan--install): Installs the appropriate packages.
* [`libreswan::service`](#libreswan--service): Ensure that the appropriate services are running.

### Defined types

* [`libreswan::connection`](#libreswan--connection): Create a connection file in the IPSEC configuration directory.
* [`libreswan::nss::init_db`](#libreswan--nss--init_db): Initializes the NSS database, sets the correct password, and configures FIPS if necessary.
* [`libreswan::nss::loadcacerts`](#libreswan--nss--loadcacerts): Adds the CA certificates to the NSS trust store.
* [`libreswan::nss::loadcerts`](#libreswan--nss--loadcerts): Load a server certificate into the NSS database.

### Data types

* [`Libreswan::ConnAddr`](#Libreswan--ConnAddr): Valid libreswan connection addresses
* [`Libreswan::IP::V4::VirtualPrivate`](#Libreswan--IP--V4--VirtualPrivate): Matches valid IPv4 CIDR Mask addresses Base Regex taken from Ruby core's Resolv::IPv4::Regex  Reference: ruby/lib/resolv.rb  Copyright 2010 T
* [`Libreswan::IP::V6::VirtualPrivate`](#Libreswan--IP--V6--VirtualPrivate): Matches valid IPv4 CIDR Mask addresses Base Regex taken from Ruby core's Resolv::IPv4::Regex  Reference: ruby/lib/resolv.rb  Copyright 2010 T
* [`Libreswan::Interfaces`](#Libreswan--Interfaces): Valid libreswan interfaces
* [`Libreswan::VirtualPrivate`](#Libreswan--VirtualPrivate): Valid virtual private addresses

## Classes

### <a name="libreswan"></a>`libreswan`

---
> It is very important you read the documentation that comes with libreswan
> before attempting to use this module.
---

---
> This module is designed to install and configure system IPSEC capabilities
> using libreswan.

> It will also configure and maintain the NSS database used by libreswan if you
> have chosen to let SIMP manage your PKI certificates.

> To add and start tunnels that will be managed by libreswan see the manifest
> `libreswan::add_connection`.
---

This module is optimally designed for use within a larger SIMP ecosystem, but
it can be used independently:

* When included within the SIMP ecosystem,
  security compliance settings will be managed from the Puppet server.

* If used independently, all SIMP-managed security subsystems are disabled by
  default, and must be explicitly opted into by administrators. Please review
  items referring to `simp_options::*` for additional information.

* See the libreswan documentation https://libreswan.org/man/ipsec.conf.5.html
  for more information regarding these variables.

* Any variable set to `undef` will not appear in the configuration file and
  will default to the value set by libreswan. Those set will appear in the
  configuration file but can be overwritten using Hiera.

* **See also**
  * https://libreswan.org

#### Parameters

The following parameters are available in the `libreswan` class:

* [`service_name`](#-libreswan--service_name)
* [`package_name`](#-libreswan--package_name)
* [`trusted_nets`](#-libreswan--trusted_nets)
* [`firewall`](#-libreswan--firewall)
* [`fips`](#-libreswan--fips)
* [`pki`](#-libreswan--pki)
* [`haveged`](#-libreswan--haveged)
* [`nssdb_password`](#-libreswan--nssdb_password)
* [`myid`](#-libreswan--myid)
* [`protostack`](#-libreswan--protostack)
* [`interfaces`](#-libreswan--interfaces)
* [`listen`](#-libreswan--listen)
* [`ikeport`](#-libreswan--ikeport)
* [`nflog_all`](#-libreswan--nflog_all)
* [`nat_ikeport`](#-libreswan--nat_ikeport)
* [`keep_alive`](#-libreswan--keep_alive)
* [`virtual_private`](#-libreswan--virtual_private)
* [`myvendorid`](#-libreswan--myvendorid)
* [`nhelpers`](#-libreswan--nhelpers)
* [`plutofork`](#-libreswan--plutofork)
* [`crlcheckinterval`](#-libreswan--crlcheckinterval)
* [`strictcrlpolicy`](#-libreswan--strictcrlpolicy)
* [`ocsp_enable`](#-libreswan--ocsp_enable)
* [`ocsp_strict`](#-libreswan--ocsp_strict)
* [`ocsp_timeout`](#-libreswan--ocsp_timeout)
* [`ocsp_uri`](#-libreswan--ocsp_uri)
* [`ocsp_trustname`](#-libreswan--ocsp_trustname)
* [`syslog`](#-libreswan--syslog)
* [`klipsdebug`](#-libreswan--klipsdebug)
* [`plutodebug`](#-libreswan--plutodebug)
* [`uniqueids`](#-libreswan--uniqueids)
* [`plutorestartoncrash`](#-libreswan--plutorestartoncrash)
* [`logfile`](#-libreswan--logfile)
* [`logappend`](#-libreswan--logappend)
* [`logtime`](#-libreswan--logtime)
* [`ddos_mode`](#-libreswan--ddos_mode)
* [`ddos_ike_treshold`](#-libreswan--ddos_ike_treshold)
* [`dumpdir`](#-libreswan--dumpdir)
* [`statsbin`](#-libreswan--statsbin)
* [`ipsecdir`](#-libreswan--ipsecdir)
* [`secretsfile`](#-libreswan--secretsfile)
* [`perpeerlog`](#-libreswan--perpeerlog)
* [`perpeerlogdir`](#-libreswan--perpeerlogdir)
* [`fragicmp`](#-libreswan--fragicmp)
* [`hidetos`](#-libreswan--hidetos)
* [`overridemtu`](#-libreswan--overridemtu)
* [`block_cidrs`](#-libreswan--block_cidrs)
* [`clear_cidrs`](#-libreswan--clear_cidrs)
* [`clear_private_cidrs`](#-libreswan--clear_private_cidrs)
* [`private_cidrs`](#-libreswan--private_cidrs)
* [`private_clear_cidrs`](#-libreswan--private_clear_cidrs)

##### <a name="-libreswan--service_name"></a>`service_name`

Data type: `String`

The name of the IPSEC service.

##### <a name="-libreswan--package_name"></a>`package_name`

Data type: `String`

The name of the libreswan package.

##### <a name="-libreswan--trusted_nets"></a>`trusted_nets`

Data type: `Simplib::Netlist`

An allowed set of subnetworks (in CIDR notataion) with permitted access
explicitly for IPSEC communication

Default value: `simplib::lookup('simp_options::trusted_nets', {'default_value' => ['127.0.0.1/32'] })`

##### <a name="-libreswan--firewall"></a>`firewall`

Data type: `Boolean`

Whether to add appropriate rules to allow IPSEC traffic to the
SIMP-controlled firewall

Default value: `simplib::lookup('simp_options::firewall', {'default_value' => false })`

##### <a name="-libreswan--fips"></a>`fips`

Data type: `Boolean`

Whether server is in FIPS mode.

* Affects cryptography allowed to be used by IPSEC.

Default value: `simplib::lookup('simp_options::fips', {'default_value' => false })`

##### <a name="-libreswan--pki"></a>`pki`

Data type: `Variant[Boolean,Enum['simp']]`

* If `'simp'`, include `simp/pki` and use `pki::copy` to manage application
  certs in `/etc/pki/simp_apps/libreswan/x509`
* If `true`, do *not* include `simp/pki`, but still use `pki::copy` to
  manage certs in `/etc/pki/simp_apps/libreswan/x509`
* If `false`, do *not* include `simp/pki` and do *not* use pki::copy to
  manage certs.  You will need to appropriately assign a subset of:
    * app_pki_dir
    * app_pki_key
    * app_pki_cert
    * app_pki_ca
    * app_pki_ca_dir

Default value: `simplib::lookup('simp_options::pki', {'default_value' => false })`

##### <a name="-libreswan--haveged"></a>`haveged`

Data type: `Boolean`

Whether to use haveged to ensure adequate entropy

Default value: `simplib::lookup('simp_options::haveged', {'default_value' => false })`

##### <a name="-libreswan--nssdb_password"></a>`nssdb_password`

Data type: `String`

Password for the NSS database used by ipsec

Default value: `simplib::passgen('nssdb_password')`

##### <a name="-libreswan--myid"></a>`myid`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--protostack"></a>`protostack`

Data type: `Enum['netkey','klips','mast']`



Default value: `'netkey'`

##### <a name="-libreswan--interfaces"></a>`interfaces`

Data type: `Optional[Libreswan::Interfaces]`



Default value: `undef`

##### <a name="-libreswan--listen"></a>`listen`

Data type: `Optional[Simplib::IP]`



Default value: `undef`

##### <a name="-libreswan--ikeport"></a>`ikeport`

Data type: `Simplib::Port`

DEPRECATED

Default value: `500`

##### <a name="-libreswan--nflog_all"></a>`nflog_all`

Data type: `Optional[Integer]`



Default value: `undef`

##### <a name="-libreswan--nat_ikeport"></a>`nat_ikeport`

Data type: `Simplib::Port`

DEPRECATED

Default value: `4500`

##### <a name="-libreswan--keep_alive"></a>`keep_alive`

Data type: `Optional[Integer]`



Default value: `undef`

##### <a name="-libreswan--virtual_private"></a>`virtual_private`

Data type: `Libreswan::VirtualPrivate`



Default value: `['%v4:10.0.0.0/8','%v4:192.168.0.0/16','%v4:172.16.0.0/12']`

##### <a name="-libreswan--myvendorid"></a>`myvendorid`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--nhelpers"></a>`nhelpers`

Data type: `Optional[Integer]`



Default value: `undef`

##### <a name="-libreswan--plutofork"></a>`plutofork`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--crlcheckinterval"></a>`crlcheckinterval`

Data type: `Optional[Integer]`



Default value: `undef`

##### <a name="-libreswan--strictcrlpolicy"></a>`strictcrlpolicy`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--ocsp_enable"></a>`ocsp_enable`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--ocsp_strict"></a>`ocsp_strict`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--ocsp_timeout"></a>`ocsp_timeout`

Data type: `Optional[Integer]`



Default value: `undef`

##### <a name="-libreswan--ocsp_uri"></a>`ocsp_uri`

Data type: `Optional[Simplib::Uri]`



Default value: `undef`

##### <a name="-libreswan--ocsp_trustname"></a>`ocsp_trustname`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--syslog"></a>`syslog`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--klipsdebug"></a>`klipsdebug`

Data type: `String`

DEPRECATED

Default value: `'none'`

##### <a name="-libreswan--plutodebug"></a>`plutodebug`

Data type: `String`



Default value: `'none'`

##### <a name="-libreswan--uniqueids"></a>`uniqueids`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--plutorestartoncrash"></a>`plutorestartoncrash`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--logfile"></a>`logfile`

Data type: `Optional[Stdlib::Absolutepath]`



Default value: `undef`

##### <a name="-libreswan--logappend"></a>`logappend`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--logtime"></a>`logtime`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--ddos_mode"></a>`ddos_mode`

Data type:

```puppet
Optional[Enum['busy',
    'unlimited','auto']]
```



Default value: `undef`

##### <a name="-libreswan--ddos_ike_treshold"></a>`ddos_ike_treshold`

Data type: `Optional[Integer]`



Default value: `undef`

##### <a name="-libreswan--dumpdir"></a>`dumpdir`

Data type: `Stdlib::Absolutepath`



Default value: `'/var/run/pluto'`

##### <a name="-libreswan--statsbin"></a>`statsbin`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--ipsecdir"></a>`ipsecdir`

Data type: `Stdlib::Absolutepath`

The directory to store all ipsec configuration information.

Default value: `'/etc/ipsec.d'`

##### <a name="-libreswan--secretsfile"></a>`secretsfile`

Data type: `Stdlib::Absolutepath`



Default value: `'/etc/ipsec.secrets'`

##### <a name="-libreswan--perpeerlog"></a>`perpeerlog`

Data type: `Optional[Enum['yes','no']]`

DEPRECATED

Default value: `undef`

##### <a name="-libreswan--perpeerlogdir"></a>`perpeerlogdir`

Data type: `Stdlib::Absolutepath`

DEPRECATED

Default value: `'/var/log/pluto/peer'`

##### <a name="-libreswan--fragicmp"></a>`fragicmp`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--hidetos"></a>`hidetos`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--overridemtu"></a>`overridemtu`

Data type: `Optional[Integer]`



Default value: `undef`

##### <a name="-libreswan--block_cidrs"></a>`block_cidrs`

Data type: `Optional[Array[Simplib::IP::V4::CIDR]]`

List of CIDRs to which communication should never be allowed

Default value: `undef`

##### <a name="-libreswan--clear_cidrs"></a>`clear_cidrs`

Data type: `Optional[Array[Simplib::IP::V4::CIDR]]`

List of CIDRs to which communication should always be in the clear

Default value: `undef`

##### <a name="-libreswan--clear_private_cidrs"></a>`clear_private_cidrs`

Data type: `Optional[Array[Simplib::IP::V4::CIDR]]`

List of CIDRs to which communication will be in the clear, or, if the other
side initiates IPSEC, use encryption

Default value: `undef`

##### <a name="-libreswan--private_cidrs"></a>`private_cidrs`

Data type: `Optional[Array[Simplib::IP::V4::CIDR]]`

List of CIDRs to which communication should always be private

Default value: `undef`

##### <a name="-libreswan--private_clear_cidrs"></a>`private_clear_cidrs`

Data type: `Array[Simplib::IP::V4::CIDR]`

List of CIDRs to which communication should be private if possible but in
the clear otherwise

Default value: `['0.0.0.0/0']`

### <a name="libreswan--config"></a>`libreswan::config`

Configures `ipsec.conf` and necessary directories.

### <a name="libreswan--config--firewall"></a>`libreswan::config::firewall`

Ensures that the required firewall rules are defined

### <a name="libreswan--config--pki"></a>`libreswan::config::pki`

Ensure that the `simp/pki` PKI certificates are loaded into the IPSEC NSS Database.

#### Parameters

The following parameters are available in the `libreswan::config::pki` class:

* [`app_pki_external_source`](#-libreswan--config--pki--app_pki_external_source)
* [`app_pki_dir`](#-libreswan--config--pki--app_pki_dir)
* [`app_pki_key`](#-libreswan--config--pki--app_pki_key)
* [`app_pki_cert`](#-libreswan--config--pki--app_pki_cert)
* [`app_pki_ca`](#-libreswan--config--pki--app_pki_ca)

##### <a name="-libreswan--config--pki--app_pki_external_source"></a>`app_pki_external_source`

Data type: `String`

* If `$pki` = `'simp'` or `true`, this is the directory from which certs
will be copied, via `pki::copy`.
* If `$pki` = `false`, this variable has no effect.

Default value: `simplib::lookup('simp_options::pki::source', { 'default_value' => '/etc/pki/simp/x509' })`

##### <a name="-libreswan--config--pki--app_pki_dir"></a>`app_pki_dir`

Data type: `Stdlib::Absolutepath`

Controls the base path of the other `app_pki_*` parameters.

Default value: `'/etc/pki/simp_apps/libreswan/x509'`

##### <a name="-libreswan--config--pki--app_pki_key"></a>`app_pki_key`

Data type: `Stdlib::Absolutepath`

Path and name of the private SSL key file

Default value: `"${app_pki_dir}/private/${facts['networking']['fqdn']}.pem"`

##### <a name="-libreswan--config--pki--app_pki_cert"></a>`app_pki_cert`

Data type: `Stdlib::Absolutepath`

Path and name of the public SSL certificate

Default value: `"${app_pki_dir}/public/${facts['networking']['fqdn']}.pub"`

##### <a name="-libreswan--config--pki--app_pki_ca"></a>`app_pki_ca`

Data type: `Stdlib::Absolutepath`

Path and name of the CA.

Default value: `"${app_pki_dir}/cacerts/cacerts.pem"`

### <a name="libreswan--config--pki--nsspki"></a>`libreswan::config::pki::nsspki`

Called when the certificates change or when the database is initialized.

#### Parameters

The following parameters are available in the `libreswan::config::pki::nsspki` class:

* [`certname`](#-libreswan--config--pki--nsspki--certname)

##### <a name="-libreswan--config--pki--nsspki--certname"></a>`certname`

Data type: `String[1]`

The name of the certificate to be used

Default value: `$facts['networking']['fqdn']`

### <a name="libreswan--install"></a>`libreswan::install`

Installs the appropriate packages.

### <a name="libreswan--service"></a>`libreswan::service`

Ensure that the appropriate services are running.

## Defined types

### <a name="libreswan--connection"></a>`libreswan::connection`

You can can set up defaults for all of your connections by using the name
'default'. This will create a file `default.conf` with a `'conn %default'`
header.  Then, all settings in default.conf will be used as defaults for
connections specified in other files.

Not all available, connection-related, libreswan settings are defined
here. However, should you need a missing setting you can manually
create a correctly-formatted, connection configuration file in the
IPSEC configuration directory.  This file must have a `.conf` suffix.

* Manually generated configuration files are not managed, or purged, by Puppet.

The following parameters correspond to libreswan settings for which
the default values are different from the libreswan defaults. You
can override the defaults by passing in different data in the
definition parameters.

The rest of the parameters map one-to-one to libreswan settings and
are `undef`.

Any `undef` parameter will not appear in the generated configuration file for
the connection. See libreswan documentation for the setting defaults when
omitted from a connection's configuration.
https://libreswan.org/man/ipsec.conf.5.html, the `CONN:SETTINGS` section

#### Parameters

The following parameters are available in the `libreswan::connection` defined type:

* [`dir`](#-libreswan--connection--dir)
* [`keyingtries`](#-libreswan--connection--keyingtries)
* [`ike`](#-libreswan--connection--ike)
* [`phase2alg`](#-libreswan--connection--phase2alg)
* [`left`](#-libreswan--connection--left)
* [`right`](#-libreswan--connection--right)
* [`connaddrfamily`](#-libreswan--connection--connaddrfamily)
* [`leftaddresspool`](#-libreswan--connection--leftaddresspool)
* [`leftsubnet`](#-libreswan--connection--leftsubnet)
* [`leftsubnets`](#-libreswan--connection--leftsubnets)
* [`leftprotoport`](#-libreswan--connection--leftprotoport)
* [`leftsourceip`](#-libreswan--connection--leftsourceip)
* [`leftupdown`](#-libreswan--connection--leftupdown)
* [`leftcert`](#-libreswan--connection--leftcert)
* [`leftrsasigkey`](#-libreswan--connection--leftrsasigkey)
* [`leftrsasigkey2`](#-libreswan--connection--leftrsasigkey2)
* [`leftsendcert`](#-libreswan--connection--leftsendcert)
* [`leftnexthop`](#-libreswan--connection--leftnexthop)
* [`leftid`](#-libreswan--connection--leftid)
* [`leftca`](#-libreswan--connection--leftca)
* [`rightid`](#-libreswan--connection--rightid)
* [`rightrsasigkey`](#-libreswan--connection--rightrsasigkey)
* [`rightrsasigkey2`](#-libreswan--connection--rightrsasigkey2)
* [`rightca`](#-libreswan--connection--rightca)
* [`rightaddresspool`](#-libreswan--connection--rightaddresspool)
* [`rightsubnets`](#-libreswan--connection--rightsubnets)
* [`rightsubnet`](#-libreswan--connection--rightsubnet)
* [`rightprotoport`](#-libreswan--connection--rightprotoport)
* [`rightsourceip`](#-libreswan--connection--rightsourceip)
* [`rightupdown`](#-libreswan--connection--rightupdown)
* [`rightcert`](#-libreswan--connection--rightcert)
* [`rightsendcert`](#-libreswan--connection--rightsendcert)
* [`rightnexthop`](#-libreswan--connection--rightnexthop)
* [`auto`](#-libreswan--connection--auto)
* [`authby`](#-libreswan--connection--authby)
* [`type`](#-libreswan--connection--type)
* [`ikev2`](#-libreswan--connection--ikev2)
* [`mobike`](#-libreswan--connection--mobike)
* [`phase2`](#-libreswan--connection--phase2)
* [`ikepad`](#-libreswan--connection--ikepad)
* [`fragmentation`](#-libreswan--connection--fragmentation)
* [`sha2_truncbug`](#-libreswan--connection--sha2_truncbug)
* [`narrowing`](#-libreswan--connection--narrowing)
* [`sareftrack`](#-libreswan--connection--sareftrack)
* [`leftxauthserver`](#-libreswan--connection--leftxauthserver)
* [`rightxauthserver`](#-libreswan--connection--rightxauthserver)
* [`leftxauthusername`](#-libreswan--connection--leftxauthusername)
* [`rightxauthusername`](#-libreswan--connection--rightxauthusername)
* [`leftxauthclient`](#-libreswan--connection--leftxauthclient)
* [`rightxauthclient`](#-libreswan--connection--rightxauthclient)
* [`leftmodecfgserver`](#-libreswan--connection--leftmodecfgserver)
* [`rightmodecfgserver`](#-libreswan--connection--rightmodecfgserver)
* [`leftmodecfgclient`](#-libreswan--connection--leftmodecfgclient)
* [`rightmodecfgclient`](#-libreswan--connection--rightmodecfgclient)
* [`xauthby`](#-libreswan--connection--xauthby)
* [`xauthfail`](#-libreswan--connection--xauthfail)
* [`modecfgpull`](#-libreswan--connection--modecfgpull)
* [`modecfgdns`](#-libreswan--connection--modecfgdns)
* [`modecfgdns1`](#-libreswan--connection--modecfgdns1)
* [`modecfgdns2`](#-libreswan--connection--modecfgdns2)
* [`modecfgdomain`](#-libreswan--connection--modecfgdomain)
* [`modecfgdomains`](#-libreswan--connection--modecfgdomains)
* [`modecfgbanner`](#-libreswan--connection--modecfgbanner)
* [`nat_ikev1_method`](#-libreswan--connection--nat_ikev1_method)
* [`dpddelay`](#-libreswan--connection--dpddelay)
* [`dpdtimeout`](#-libreswan--connection--dpdtimeout)
* [`dpdaction`](#-libreswan--connection--dpdaction)

##### <a name="-libreswan--connection--dir"></a>`dir`

Data type: `Stdlib::Absolutepath`

The absolute path to the IPSEC configuration directory.

Default value: `'/etc/ipsec.d'`

##### <a name="-libreswan--connection--keyingtries"></a>`keyingtries`

Data type: `Integer`

The number of times a connection will try to reconnect before exiting.

Default value: `10`

##### <a name="-libreswan--connection--ike"></a>`ike`

Data type: `String`

The ciphers used in the connection.

Default value: `'aes-sha2'`

##### <a name="-libreswan--connection--phase2alg"></a>`phase2alg`

Data type: `String`

The ciphers used in the second part of the connection.

Default value: `'aes-sha2'`

##### <a name="-libreswan--connection--left"></a>`left`

Data type: `Optional[Libreswan::ConnAddr]`



Default value: `undef`

##### <a name="-libreswan--connection--right"></a>`right`

Data type: `Optional[Libreswan::ConnAddr]`



Default value: `undef`

##### <a name="-libreswan--connection--connaddrfamily"></a>`connaddrfamily`

Data type: `Optional[Enum['ipv4','ipv6']]`



Default value: `undef`

##### <a name="-libreswan--connection--leftaddresspool"></a>`leftaddresspool`

Data type: `Optional[Array[Simplib::IP,2,2]]`



Default value: `undef`

##### <a name="-libreswan--connection--leftsubnet"></a>`leftsubnet`

Data type:

```puppet
Optional[Variant[
    Enum['%no','%priv'],
    Pattern['^vhost:*'],
    Pattern['^vnet:*'],
    Simplib::IP::CIDR]]
```



Default value: `undef`

##### <a name="-libreswan--connection--leftsubnets"></a>`leftsubnets`

Data type: `Optional[Array[Simplib::IP::CIDR]]`



Default value: `undef`

##### <a name="-libreswan--connection--leftprotoport"></a>`leftprotoport`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--leftsourceip"></a>`leftsourceip`

Data type: `Optional[Simplib::IP]`



Default value: `undef`

##### <a name="-libreswan--connection--leftupdown"></a>`leftupdown`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--leftcert"></a>`leftcert`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--leftrsasigkey"></a>`leftrsasigkey`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--leftrsasigkey2"></a>`leftrsasigkey2`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--leftsendcert"></a>`leftsendcert`

Data type:

```puppet
Optional[Enum['yes', 'no',
    'never','always','sendifasked']]
```



Default value: `undef`

##### <a name="-libreswan--connection--leftnexthop"></a>`leftnexthop`

Data type:

```puppet
Optional[Variant[
    Enum['%direct','%defaultroute'],
    Simplib::IP]]
```



Default value: `undef`

##### <a name="-libreswan--connection--leftid"></a>`leftid`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--leftca"></a>`leftca`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--rightid"></a>`rightid`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--rightrsasigkey"></a>`rightrsasigkey`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--rightrsasigkey2"></a>`rightrsasigkey2`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--rightca"></a>`rightca`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--rightaddresspool"></a>`rightaddresspool`

Data type: `Optional[Array[Simplib::IP,2,2]]`



Default value: `undef`

##### <a name="-libreswan--connection--rightsubnets"></a>`rightsubnets`

Data type: `Optional[Array[Simplib::IP::CIDR]]`



Default value: `undef`

##### <a name="-libreswan--connection--rightsubnet"></a>`rightsubnet`

Data type:

```puppet
Optional[Variant[
    Enum['%no','%priv'],
    Pattern['^vhost:*'],
    Pattern['^vnet:*'],
    Simplib::IP::CIDR]]
```



Default value: `undef`

##### <a name="-libreswan--connection--rightprotoport"></a>`rightprotoport`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--rightsourceip"></a>`rightsourceip`

Data type: `Optional[Simplib::IP]`



Default value: `undef`

##### <a name="-libreswan--connection--rightupdown"></a>`rightupdown`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--rightcert"></a>`rightcert`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--rightsendcert"></a>`rightsendcert`

Data type:

```puppet
Optional[Enum['yes', 'no',
    'never','always','sendifasked']]
```



Default value: `undef`

##### <a name="-libreswan--connection--rightnexthop"></a>`rightnexthop`

Data type:

```puppet
Optional[Variant[
    Enum['%direct','%defaultroute'],
    Simplib::IP]]
```



Default value: `undef`

##### <a name="-libreswan--connection--auto"></a>`auto`

Data type:

```puppet
Optional[Enum['add','start',
    'ondemand', 'ignore']]
```



Default value: `undef`

##### <a name="-libreswan--connection--authby"></a>`authby`

Data type:

```puppet
Optional[Enum['rsasig','secret',
    'secret|rsasig', 'never', 'null']]
```



Default value: `undef`

##### <a name="-libreswan--connection--type"></a>`type`

Data type:

```puppet
Optional[Enum['tunnel','transport',
    'passthough','reject','drop']]
```



Default value: `undef`

##### <a name="-libreswan--connection--ikev2"></a>`ikev2`

Data type:

```puppet
Optional[Enum['insist','permit',
    'propose','never','yes', 'no']]
```



Default value: `undef`

##### <a name="-libreswan--connection--mobike"></a>`mobike`

Data type: `Optional[Enum['yes', 'no']]`



Default value: `undef`

##### <a name="-libreswan--connection--phase2"></a>`phase2`

Data type: `Optional[Enum['esp', 'ah']]`



Default value: `undef`

##### <a name="-libreswan--connection--ikepad"></a>`ikepad`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--connection--fragmentation"></a>`fragmentation`

Data type: `Optional[Enum['yes','no','force']]`



Default value: `undef`

##### <a name="-libreswan--connection--sha2_truncbug"></a>`sha2_truncbug`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--connection--narrowing"></a>`narrowing`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--connection--sareftrack"></a>`sareftrack`

Data type:

```puppet
Optional[Enum['yes','no',
    'conntrack']]
```



Default value: `undef`

##### <a name="-libreswan--connection--leftxauthserver"></a>`leftxauthserver`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--connection--rightxauthserver"></a>`rightxauthserver`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--connection--leftxauthusername"></a>`leftxauthusername`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--rightxauthusername"></a>`rightxauthusername`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--leftxauthclient"></a>`leftxauthclient`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--connection--rightxauthclient"></a>`rightxauthclient`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--connection--leftmodecfgserver"></a>`leftmodecfgserver`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--connection--rightmodecfgserver"></a>`rightmodecfgserver`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--connection--leftmodecfgclient"></a>`leftmodecfgclient`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--connection--rightmodecfgclient"></a>`rightmodecfgclient`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--connection--xauthby"></a>`xauthby`

Data type:

```puppet
Optional[Enum['file','pam',
    'alwaysok']]
```



Default value: `undef`

##### <a name="-libreswan--connection--xauthfail"></a>`xauthfail`

Data type: `Optional[Enum['hard','soft']]`



Default value: `undef`

##### <a name="-libreswan--connection--modecfgpull"></a>`modecfgpull`

Data type: `Optional[Enum['yes','no']]`



Default value: `undef`

##### <a name="-libreswan--connection--modecfgdns"></a>`modecfgdns`

Data type: `Optional[Array[Simplib::IP]]`

Support 3.23+ DNS configuration

Default value: `undef`

##### <a name="-libreswan--connection--modecfgdns1"></a>`modecfgdns1`

Data type: `Optional[Simplib::IP]`

Support <= 3.22 domain configuration

Default value: `undef`

##### <a name="-libreswan--connection--modecfgdns2"></a>`modecfgdns2`

Data type: `Optional[Simplib::IP]`

Support <= 3.22 domain configuration

Default value: `undef`

##### <a name="-libreswan--connection--modecfgdomain"></a>`modecfgdomain`

Data type: `Optional[String]`

Support <= 3.22 domain configuration

Default value: `undef`

##### <a name="-libreswan--connection--modecfgdomains"></a>`modecfgdomains`

Data type: `Optional[Array[String]]`

Support 3.23+ domains configuration

Default value: `undef`

##### <a name="-libreswan--connection--modecfgbanner"></a>`modecfgbanner`

Data type: `Optional[String]`



Default value: `undef`

##### <a name="-libreswan--connection--nat_ikev1_method"></a>`nat_ikev1_method`

Data type:

```puppet
Optional[Enum['drafts','rfc',
    'both']]
```



Default value: `undef`

##### <a name="-libreswan--connection--dpddelay"></a>`dpddelay`

Data type: `Optional[Pattern[/\d+[smh]$/]]`



Default value: `undef`

##### <a name="-libreswan--connection--dpdtimeout"></a>`dpdtimeout`

Data type: `Optional[Pattern[/\d+[smh]$/]]`



Default value: `undef`

##### <a name="-libreswan--connection--dpdaction"></a>`dpdaction`

Data type:

```puppet
Optional[Enum['hold', 'clear',
    'restart']]
```



Default value: `undef`

### <a name="libreswan--nss--init_db"></a>`libreswan::nss::init_db`

Initializes the NSS database, sets the correct password, and configures FIPS if necessary.

#### Parameters

The following parameters are available in the `libreswan::nss::init_db` defined type:

* [`dbdir`](#-libreswan--nss--init_db--dbdir)
* [`password`](#-libreswan--nss--init_db--password)
* [`destroyexisting`](#-libreswan--nss--init_db--destroyexisting)
* [`fips`](#-libreswan--nss--init_db--fips)
* [`token`](#-libreswan--nss--init_db--token)
* [`nsspassword`](#-libreswan--nss--init_db--nsspassword)

##### <a name="-libreswan--nss--init_db--dbdir"></a>`dbdir`

Data type: `Stdlib::Absolutepath`

Directory where the NSS database will be created.

##### <a name="-libreswan--nss--init_db--password"></a>`password`

Data type: `String`

Password used to protect the database.

* Each NSS database is broken up into tokens used for different types of
certificates, Smart cards, FIPS compliant, non-FIPS. This util sets the
FIPS and non-FIPS token to they same password.  The tokens are defined by
`$libreswan::nsstoken`. You can add tokens to array if there are other
parts of the database you want to protect.

##### <a name="-libreswan--nss--init_db--destroyexisting"></a>`destroyexisting`

Data type: `Boolean`

If true, it will remove the existing database before running the init command.

Default value: `false`

##### <a name="-libreswan--nss--init_db--fips"></a>`fips`

Data type: `Boolean`



Default value: `simplib::lookup('simp_options::fips', { 'default_value' => false})`

##### <a name="-libreswan--nss--init_db--token"></a>`token`

Data type: `String`



Default value: `'NSS Certificate DB'`

##### <a name="-libreswan--nss--init_db--nsspassword"></a>`nsspassword`

Data type: `Stdlib::Absolutepath`



Default value: `"${dbdir}/nsspassword"`

### <a name="libreswan--nss--loadcacerts"></a>`libreswan::nss::loadcacerts`

Adds the CA certificates to the NSS trust store.

#### Parameters

The following parameters are available in the `libreswan::nss::loadcacerts` defined type:

* [`dbdir`](#-libreswan--nss--loadcacerts--dbdir)
* [`nsspwd_file`](#-libreswan--nss--loadcacerts--nsspwd_file)
* [`cert`](#-libreswan--nss--loadcacerts--cert)
* [`token`](#-libreswan--nss--loadcacerts--token)
* [`certtype`](#-libreswan--nss--loadcacerts--certtype)

##### <a name="-libreswan--nss--loadcacerts--dbdir"></a>`dbdir`

Data type: `Stdlib::Absolutepath`

The directory where the DB is located

##### <a name="-libreswan--nss--loadcacerts--nsspwd_file"></a>`nsspwd_file`

Data type: `Stdlib::Absolutepath`



Default value: `"${dbdir}/nsspassword"`

##### <a name="-libreswan--nss--loadcacerts--cert"></a>`cert`

Data type: `Stdlib::Absolutepath`

The absolute path to the public portion CA certificate.

##### <a name="-libreswan--nss--loadcacerts--token"></a>`token`

Data type: `String`



Default value: `'NSS Certificate DB'`

##### <a name="-libreswan--nss--loadcacerts--certtype"></a>`certtype`

Data type: `Enum['PEM','DER']`

The format the certificate is in. PEM and DER are currently acceptable.

Default value: `'PEM'`

### <a name="libreswan--nss--loadcerts"></a>`libreswan::nss::loadcerts`

Load a server certificate into the NSS database.

#### Parameters

The following parameters are available in the `libreswan::nss::loadcerts` defined type:

* [`dbdir`](#-libreswan--nss--loadcerts--dbdir)
* [`nsspwd_file`](#-libreswan--nss--loadcerts--nsspwd_file)
* [`cert`](#-libreswan--nss--loadcerts--cert)
* [`token`](#-libreswan--nss--loadcerts--token)
* [`key`](#-libreswan--nss--loadcerts--key)
* [`certtype`](#-libreswan--nss--loadcerts--certtype)

##### <a name="-libreswan--nss--loadcerts--dbdir"></a>`dbdir`

Data type: `Stdlib::Absolutepath`

The directory where the NSS Database is located.

##### <a name="-libreswan--nss--loadcerts--nsspwd_file"></a>`nsspwd_file`

Data type: `Stdlib::Absolutepath`

The file which contains the password if there is one.

Default value: `"${dbdir}/nsspassword"`

##### <a name="-libreswan--nss--loadcerts--cert"></a>`cert`

Data type: `Stdlib::Absolutepath`

The absolute path to the public portion of the cert.

##### <a name="-libreswan--nss--loadcerts--token"></a>`token`

Data type: `String`



Default value: `'NSS Certificate DB'`

##### <a name="-libreswan--nss--loadcerts--key"></a>`key`

Data type: `Optional[Stdlib::Absolutepath]`

The absolute path to the private portion of the cert.

Default value: `undef`

##### <a name="-libreswan--nss--loadcerts--certtype"></a>`certtype`

Data type: `Enum['PEM','P12']`

The format the certificate is in.

Default value: `'PEM'`

## Data types

### <a name="Libreswan--ConnAddr"></a>`Libreswan::ConnAddr`

Valid libreswan connection addresses

Alias of

```puppet
Variant[Enum[
    '%any',
    '%defaultroute',
    '%opportunistic',
    '%opportunisticgroup',
    '%group'
  ], Simplib::IP::V4, Simplib::IP::V6, Pattern['^%[a-zA-Z]+\d+$']]
```

### <a name="Libreswan--IP--V4--VirtualPrivate"></a>`Libreswan::IP::V4::VirtualPrivate`

Matches valid IPv4 CIDR Mask addresses
Base Regex taken from Ruby core's Resolv::IPv4::Regex

Reference: ruby/lib/resolv.rb

Copyright 2010 Tanaka Akira <kr@fsij.org>
Released under the guidance of the Ruby COPYING file section 2(a)
Commit 4e3a98d383eb3c420df5208d83f9aba70b504e33

Alias of `Pattern['^(?-mix:\A%v4:(!)?((?x-mi:0|1(?:[0-9][0-9]?)?|2(?:[0-4][0-9]?|5[0-5]?|[6-9])?|[3-9][0-9]?))\.((?x-mi:0|1(?:[0-9][0-9]?)?|2(?:[0-4][0-9]?|5[0-5]?|[6-9])?|[3-9][0-9]?))\.((?x-mi:0|1(?:[0-9][0-9]?)?|2(?:[0-4][0-9]?|5[0-5]?|[6-9])?|[3-9][0-9]?))\.((?x-mi:0|1(?:[0-9][0-9]?)?|2(?:[0-4][0-9]?|5[0-5]?|[6-9])?|[3-9][0-9]?))/(3[012]|[12][0-9]|[0-9])\z)$']`

### <a name="Libreswan--IP--V6--VirtualPrivate"></a>`Libreswan::IP::V6::VirtualPrivate`

Matches valid IPv4 CIDR Mask addresses
Base Regex taken from Ruby core's Resolv::IPv4::Regex

Reference: ruby/lib/resolv.rb

Copyright 2010 Tanaka Akira <kr@fsij.org>
Released under the guidance of the Ruby COPYING file section 2(a)
Commit 4e3a98d383eb3c420df5208d83f9aba70b504e33

Alias of `Pattern['^(?x-mi:(\A%v6:(!)?(?x-mi:(?:(?x-mi:(?:[0-9A-Fa-f]{1,4}:){7}[0-9A-Fa-f]{1,4}/(12[0-8]|1[01][0-9]|[0-9]?[0-9])\z))|(?:(?x-mi:((?:[0-9A-Fa-f]{1,4}(?::[0-9A-Fa-f]{1,4})*)?)::((?:[0-9A-Fa-f]{1,4}(?::[0-9A-Fa-f]{1,4})*)?)/(12[0-8]|1[01][0-9]|[0-9]?[0-9])\z))|(?:(?x-mi:((?:[0-9A-Fa-f]{1,4}:){6,6})(\d+)\.(\d+)\.(\d+)\.(\d+)/(12[0-8]|1[01][0-9]|[0-9]?[0-9])\z))|(?:(?x-mi:((?:[0-9A-Fa-f]{1,4}(?::[0-9A-Fa-f]{1,4})*)?)::((?:[0-9A-Fa-f]{1,4}:)*)(\d+)\.(\d+)\.(\d+)\.(\d+)/(12[0-8]|1[01][0-9]|[0-9]?[0-9]))))\z))$']`

### <a name="Libreswan--Interfaces"></a>`Libreswan::Interfaces`

Valid libreswan interfaces

Alias of

```puppet
Array[Variant[
    Enum['%none','%defaultroute'],
    Pattern['(\w+=\w+)']
  ]]
```

### <a name="Libreswan--VirtualPrivate"></a>`Libreswan::VirtualPrivate`

Valid virtual private addresses

Alias of

```puppet
Array[Variant[
    Libreswan::IP::V4::VirtualPrivate,
    Libreswan::IP::V6::VirtualPrivate
  ]]
```

