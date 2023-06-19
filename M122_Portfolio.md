# Datei-Sortierungs Skript
$In diesem Projekt geht es darum ein Skript zu erstellen, welches etwas auf dem Laptop automatisiert. Also habe ich mir gedacht, dass es sehr praktisch wäre ein Skript zu haben, welches mir automatisch die Dateien einsortiert, die ich von Moodle herunterlade. Dies bedeutet, dass das Skript die gewollten Dateien erkennt, in einen bestimmten Ordner sortiert und dies natürlich ohne Fehler.

## Inhalt
Wie gesagt geht es in diesem Projekt darum 







```ps
 $logFile = "C:\Users\timog\OneDrive - BBBaden\M122\GoedertierTimo_LB_M122_2021-V3\fileMovement.log"
 $currentDate = Get-Date -Format "dd.MM.yyyy"
 $logMessage = "$currentDate - File $($file.Name) was moved to $destinationFolder"
 Add-Content -Path $logFile -Value $logMessage
```
Hier habe ich noch einen kleinen Zusatz gemacht, in dem es eine Linie erstellt für jede einsortierte Datei, in der Datei fileMovement.log.

## Was habe ich in diesem Auftrag gelernt?
Ich habe gelernt, wie man Dateien, mit Powershell Befehlen, bewegt. Dies geht mit dem Behfel Move-Item. Ich habe dies so benutzt:
```ps
 Move-Item -Path <Dateiname> -Destination <Zielort>
```

## Was das Programm zeigt
Das Programm macht keine Ausgaben, nur eine Log Message für jede einsortierte Datei.

## Selbstreflexion
Ich hatte am Anfang ein Problem, da ich nicht wusste, was ich machen sollte. Als ich dann aber eine Idee hatte, ging alles schnell, da ich ein konkretes Ziel vor Augen hatte. Während dem Programmieren hat mit GPT auch viel geholfen, obwohl es am Anfang ein bisschen zu viel war. Während dem Arbeiten konnte ich mich auch gut konzentrieren und war sehr wenig abgelenkt, vorallem im Vergleich zu vorherigen Projekten. 

## Reflexion
### Was habe ich gut gemacht?

-Ich konnte mich gut konzentrieren während dem Arbeiten, weil ich ein konkretes Ziel vor Augen hatte.

-Ich habe meine Idee immer wieder ein bisschen erweitert, wenn ich fertig war, mit dem Teil, den ich mir vorgenommen hatte.

### Was habe ich weniger gut gemacht?

-Ich habe sehr lange überlegt, was ich machen soll und habe deswegen Zeit verloren.

-Am Anfang habe ich noch viel mit GPT gemacht, weil ich nicht wusste, wie ich anfangen soll.
