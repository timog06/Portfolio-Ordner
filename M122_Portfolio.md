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


## Was das Programm zeigt
Das Programm macht keine Ausgaben, nur eine Log Message für jede einsortierte Datei.

## Selbstreflexion


## Reflexion
### Was habe ich gut gemacht?

-Ich konnte mich gut konzentrieren während dem Arbeiten, weil ich ein konkretes Ziel vor Augen hatte.

-Ich habe meine Idee immer wieder ein bisschen erweitert, wenn ich fertig war, mit dem Teil, den ich mir vorgenommen hatte.

### Was habe ich weniger gut gemacht?

-Ich habe sehr lange überlegt, was ich machen soll und habe deswegen Zeit verloren.

-Am Anfang habe ich noch viel mit GPT gemacht, weil ich nicht wusste, wie ich anfangen soll.
