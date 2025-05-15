
### 1. Síťová konfigurace Zabbix appliance

Nastavte statickou IP adresu na LAN rozhraní (např. `eth1`):

```bash
cd /etc/sysconfig/network-scripts
vi ifcfg-eth1
```

Příklad konfigurace:

```
DEVICE="eth1"
BOOTPROTO=none
ONBOOT="yes"
NM_CONTROLLED="no"
TYPE=Ethernet
NAME="eth1"
IPADDR=192.168.1.2
PREFIX=24
DEFROUTE=no
PEERDNS=no
PEERROUTES=no
```

Restartujte síť:

```bash
systemctl restart network.service
```

### 2. Ověření konektivity

Ověřte, že Zabbix appliance dosáhne pfSense:

```bash
ping 192.168.1.1
```

Ověřte SNMP komunikaci:

```bash
snmpwalk -v2c -c public 192.168.1.1 system
```

### 3. Import Zabbix šablony a hosta

- Importujte šablonu **PFSense by SNMP** do Zabbixu.
- Přidejte hosta pfSense do skupiny FreeBSD servers a přiřaďte šablonu.
- Nastavte SNMP rozhraní hosta s IP adresou pfSense a komunitou.

## Použití

Po správné konfiguraci by měl Zabbix automaticky sbírat data z pfSense a zobrazovat je v grafech a přehledech.

## Troubleshooting

- Pokud nejsou v grafech žádná data, zkontrolujte:
  - Správnost SNMP komunity v makrech (např. `{$SNMP_COMMUNITY}`).
  - Připojení mezi Zabbix appliance a pfSense.
  - Import šablony do Zabbixu.
- Pro testování SNMP lze použít příkaz `snmpwalk`.

