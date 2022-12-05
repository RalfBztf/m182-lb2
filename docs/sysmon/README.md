# Sysmon

## Recherche

Sysmon heisst System Monitor und die neuste Version ist aktuell v14.12 (10.11.2022)
Mithilfe von Sysmon kann das ganze System protokolliert werden. (Dienste, Registry-Einträge, Zugriffe etc.) Es hilft bei Fehlersuchen oder beim aufspüren von Malware-Aktivitäten. Es kann z.B nach bestimmten Events gesucht werden und mit Conditions verglichen werden. (is, contains, less than etc.)
Weitere hilfreiche Informationen auf [Microsoft](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

## Installation

### Manuell

1. [Download](https://download.sysinternals.com/files/Sysmon.zip)
2. Ein Konfigurationsfile erstellen / herunterladen
3. sysmon64.exe und config.xml nach C:\Windows verschieben
4. C:\Windows\sysmon64.exe –accepteula –i c:\windows\config.xml

### Automatisiert

Auf unseren Systemen ist Sysmon mittels Vagrantfile direkt mitinstalliert worden.
Es gäbe auch die Möglichkkeit über ein Softwareverteilungssystem wie SCCM zu machen oder auch per GPO.


### Konfiguration

Auf z.B dem DC ist Sysmon (inkl. Configfile) in ```C:\ProgramData\Sysmon``` installiert.

### Konfigurationsfile

Das Konfigurationsfile ist in der Schemaversion 4.60. 3 Hauptoptionen wurden am Anfang definiert:

```sysmon
<HashAlgorithms>*</HashAlgorithms> <!-- Sämtliche Hashalgorithmen werden verwendet -->
<CheckRevocation>False</CheckRevocation> <!-- Signaturen werden nich überprüft -->
<DnsLookup>False</DnsLookup> <!-- IP Adressen werden nicht aufgelöst -->
```

Innerhalb vom <Sysmon>-Tag wird der Tag <EventFiltering> definiert. Darin können <RuleGroups>-Tags erstellt werden in der Sachen überwacht werden können. (ProcessCreate, ProcessTerminate, NetworkConnect etc.) Mit ```onmactch=``` kann angegeben werden, ob die Filterbedingung bei Zutreffen inkludiert oder exkludiert wird. Z.B bei NetworkConnect macht es sinn Cloud-Dienste wie OneDrive, Dropbox etc. zu exkludieren.

### Nutzung

Sysmon Logs zu finden unter: **Application and Services --> Microsoft --> Windows --> Sysmon --> Operational**
