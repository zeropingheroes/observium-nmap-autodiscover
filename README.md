# Observium nmap Autodiscover

Automatically scan for devices running SNMP in the same subnet and add them to Observium

## Requirements

- Ubuntu Server 16.04
- Observium installed locally

## Installation

1. `sudo apt update -y` 
2. `sudo apt install -y nmap`
3. `git clone https://github.com/zeropingheroes/observium-nmap-autodiscover.git && cd observium-nmap-autodiscover`

## Configuration

### Observium

When `observium-nmap-autodiscover.sh` attempts to add a device to Observium, no SNMP auth details are given.
This means they must be set in `/opt/observium/config.php` so that Observium will try to authenticate with 
the device using those details. 

Documentation: http://docs.observium.org/config_options/#snmp-settings

#### Example `/opt/observium/config.php`

    <?php
    
    // [... the rest of your Observium config ...]

    // SNMPv1 & SNMPv2 communities
    $config['snmp']['community'][] = "public";
    $config['snmp']['community'][] = "my-secret-community-name-configured-on-my-devices";
    $config['snmp']['community'][] = "some-other-community";

    // SNMPv3c user #1
    $config['snmp']['v3'][0]['authlevel'] = "noAuthNoPriv";
    $config['snmp']['v3'][0]['authname'] = "observium";
    $config['snmp']['v3'][0]['authpass'] = "";
    $config['snmp']['v3'][0]['authalgo'] = "MD5";
    $config['snmp']['v3'][0]['cryptopass'] = "";
    $config['snmp']['v3'][0]['cryptoalgo'] = "AES";

    // SNMPv3c user #2
    $config['snmp']['v3'][1]['authlevel'] = "authNoPriv";
    $config['snmp']['v3'][1]['authname'] = "vendor";
    $config['snmp']['v3'][1]['authpass'] = "supersecret";
    $config['snmp']['v3'][1]['authalgo'] = "SHA";
    $config['snmp']['v3'][1]['cryptopass'] = "";
    $config['snmp']['v3'][1]['cryptoalgo'] = "DES";


### observium-nmap-autodiscover.sh

If Observium is installed in a directory other than `/opt/observium` then run:

`export OBSERVIUM_INSTALL_DIR=/your/observium/location`

## Usage

`sudo ./observium-nmap-autodiscover.sh`

