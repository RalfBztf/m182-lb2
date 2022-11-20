# WSUS

* Teils Server haben autologin???

* CNAME erstellt für die eine GPO
* gpupdate /force gemacht

## Recherche

WSUS wird verwendet, um für Windows spezifische Update freizugeben. Dazu gehören Updates für Microsoft Produkte wie Defender oder .NET Framework, die montalichen Kumulativen Updates, Feature Updates oder auch Treiber Updates.

WSUS Alternativen wären SCCM, Ivanti Securiry Controls oder Matrix42 Patch Management. Auch viele andere Softwareverteilungssysteme bieten das verteilen von Updates an.

Wenn man die PowerShell als Admin auf dem WSUS-Server ausführt, kann man diverse PowerShell Befehle nutzen, um den WSUS zu steuern. Um eine Synchronisation zu steuern braucht man nur folgendes auszuführen:

```powershell
(Get-WsusServer).GetSubscription().StartSynchronization()
```

Alle Computer auslesen:

```powershell
Get-WsusComputer
```

Viele weitere cmdlets zum Freigeben oder Ablehnen von Updates unter [business.com](https://www.business.com/articles/powershell-windows-software-update-services/)

Für Linux gäbe es die Möglichkeit einen lokalen Mirror zu hosten, welcher die Updates von einem öffentlichen bezieht. Für die Clients trägt man dann diesen lokalen mirrot ein.

## Installation

Die Installation für einen WSUS kann man einfach in einem Server-Manager vornehmen. Dazu wählt man den Dienst ```Windows Server Update Services``` aus.

![wsus-serverrole](screenshots/wsus-install/wsus-serverrole.png)
Der IIS-Webserver wird automatisch ausgewählt, da Updates mithilfe des IIS verteilt werden.

![features](screenshots/wsus-install/features.png)
Die Features so belassen, wie sie sind.

![role-services](screenshots/wsus-install/role-services.png)
WID-Connectivity (Windows Internal Database Verbindung) ist die Datenbank des WSUS. Anstelle von WID könnte auch SQL-Server ausgewählt werden.

![content](screenshots/wsus-install/content.png)
Hier kann eine Platte ausgewählt werden, wo die Updates gespeichert werden. In meinem Fall habe ich eine Platte mit 30GB angelegt.

![iis-roles](screenshots/wsus-install/iis-roles.png)

Die Rollen von IIS so belassen, wie sie sind.

## Konfiguration

### Initiale Konfiguration

![before-you-begin](screenshots/wsus-config/before-you.begin.png)

1. Clients sind im selben Netz wir der WSUS
2. Server darf zu Microsoft verbinden
3. Proxy-Server haben wir nicht

![improvement-program](screenshots/wsus-config/improvement-program.png)
Hacken deaktivieren und Next

![upstream-server](screenshots/wsus-config/upstream-server.png)
Auswählen, dass von Microsoft synchronisiert wird

![proxy](screenshots/wsus-config/proxy-server.png)
Wir verwenden keinen Proxy, so muss auch beim WSUS nichts konfiguriert werden. Falls ein Proxy im Unternehmen verwendet wird, muss dies konfiguriert werden. Weitere Infos bei [Microsoft](https://learn.microsoft.com/fr-fr/security-updates/WindowsUpdateServices/18127272)

![languages](screenshots/wsus-config/language-settings.png)
Die auf **Nur English** belassen.

![products](screenshots/wsus-config/choose-products.png)
Folgende, für meine Infrastruktur notwendige Produkte, habe ich ausgewählt:

* Microsoft Advanced Threat Analytics
* Microsoft Defender Antivirus
* Microsoft Defender for Endpoint
* Microsoft Endpoint
* Windows 10
* Windows 10 and later drivers
* Windows 10, version 1903 and later
* Windows 10, version 1903 and later, Upgrade & Servicing Drivers
* Windows Server 2016 and Later Servicing Drivers
* Windows Server 2016

![classification](screenshots/wsus-config/classifications.png)
Vorerst mal auf Default lassen

![sync](screenshots/wsus-config/automatic-sync.png)
Es empfiehlt sich die Synchronisation Nachts laufen zu lassen, um nicht den Server und das Netzwerk am Tag zu überlasten.

![finish](screenshots/wsus-config/finish.png)
Intitiale Synchronisierung beginnen.

![end](screenshots/wsus-config/end.png)
Finish. Group Policy und Computer Gruppen werden später definiert.

### Nach KOonfigurationen

Es empfiehlt sich in WSUS Gruppen anzulegen. Man kann Updates nur für bestimmte Gruppen freigeben und so z.B eine Testgruppe machen. Ich habe nun den einen Windows Server 2016 in WinServer2016_test genommen und den DC in WinServer2016Prod.

![wsus-groups](screenshots/wsus-groups.png)

Nachträglich hab ich gemerkt, dass die Klassifikation **Updates** und **Drivers** bei WSUS aktiviert werden muss, um auch .NET Framework Updates und Treiber zu erhalten. Dazu kann in den Settings unter ```Products and Classification``` Updates, sowie Drivers aktiviert werden. Danach muss eine Synchronisation gestartet werden.

![enable](screenshots/enable-driver-and-update.png)

Nun sind alles Updates Synchronisiert und stehen zur Freigabe / Ablehnung bereit.


## Testing und Reporting

![update-report](screenshots/update-report.png)

Hier sieht man das ```2022-09 Servicing Stack Update for Windows Server 2016...``` das neuste SSU für WinServer2016 ist. Das SSU Updated die Windows Update Engine selbst. An Anhanf von diesem Baum in der 2. Spalte kann erkannt werden, ob das Update neu oder bereits superseded (durch ein neueres abgelöst) ist. Unten im Diagramm sieht man das 2