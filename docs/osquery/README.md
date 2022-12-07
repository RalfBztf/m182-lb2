# OSQuery

## Client Registrierung

Von [Github](https://github.com/kolide/launcher/releases/download/v0.12.1/windows-binaries.zip) das ZIP herunterladen und in das Verzeichnis gehen wo die ```launcher.exe``` liegt.

Folgender Befehl ausführen, um den DC an der Kolide Software bekannt zu machen:

```cmd
.\launcher.exe --enroll_secret=enrollmentsecret --hostname=logger:8412 --root_directory="C:\\Program Files\\osquery" --insecure
```

CMD schliessen und den Dienst "osqueryId" starten.

## Recherche

OSquery kann diverse Information vom Betriebsystem auslesen. (Hostname, IP, MAC-Adresse, CPU, RAM, Registry-Keys etc.) Diese werden in einer Datenbank gespeichert und können einfach mit SQL abgefragt werden. 
OSQuery ist aktuell in der Version 5.6.0 (Stand 10. Oktober 2022).  
Ein Use-Case ist z.B Registry-Keys zu überwachen, ob ein Server die Updates vom richtigen Server bezieht. Man kann auch installierte Software überwachen.

## Konfiguration / Testing