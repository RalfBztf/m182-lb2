# Sysmon

## Recherche

Sysmon heisst System Monitor und die neuste Version ist aktuell v14.12 (10.11.2022)
Mithilfe von Sysmon kann das ganze System protokolliert werden. Es hilft bei Fehlersuchen oder Malware-Aktivitäten. Es kann z.B nach bestimmten Events gesucht werden und mit Conditions verglichen werden. (is, contains, less than etc.)
Weitere hilfreiche Informationen auf [Microsoft](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

## Installation

### Manuell

1. [Download](https://download.sysinternals.com/files/Sysmon.zip)
2. Ein Konfigurationsfile erstellen / herunterladen
3. config.xml nach C:\Windows verschieben
4. sysmon64.exe –accepteula –i c:\windows\config.xml

### Automatisiert

Komplett automatisiert installieren kann man es, wenn der Server in in einem Softwareverteilungssystem ist. (Ebenfalls mit dem Parameter von oben)
Eine Alternative wäre per GPO. Action: **Start a program** ```\\dc\SYSVOL\windomain.local\scripts\Sysmon\Sysmon.exe```
**Add arguments (Optional)** ```-accepteula -i \\dc01\SYSVOL\windomain.local\scripts\Sysmon\config.xml```

### Konfiguration



### Nutzung

Sysmon Logs zu finden unter: **Application and Services --> Microsoft --> Windows --> Sysmon --> Operational**