# Datei-Sortierungs Skript
$In diesem Projekt geht es darum ein Skript zu erstellen, welches etwas auf dem Laptop automatisiert. Also habe ich mir gedacht, dass es sehr praktisch wäre ein Skript zu haben, welches mir automatisch die Dateien einsortiert, die ich von Moodle herunterlade. Dies bedeutet, dass das Skript die gewollten Dateien erkennt, in einen bestimmten Ordner sortiert und dies natürlich ohne Fehler.

## Inhalt
Wie gesagt habe ich in diesem Projekt einen Datei-Sortier Skript gemacht. Natürlich musste ich mich zuerst informieren, um überhaupt zu wissen, wie ich das mache. Das erste, welches ich gemacht habe, war GPT zu fragen, wie er es machen würde. Da habe ich gesehen, welche Befehle er benutzt. Um dies dann zu erweitern habe ich im Internet nachgeschaut wie dies geht.

Als ich dann den ersten Teil fertig hatte sah es so aus:
```ps
# Define the destination folder
$destinationPath = "C:\Users\timog\OneDrive - BBBaden"

# Do it for each file
foreach ($file in $files) {

    #Folder names and variables for the next step
    $aufträgeDir = Join-Path -Path $baseDir -ChildPath "Aufträge"
    $präsentationenDir = Join-Path -Path $baseDir -ChildPath "Präsentationen"
    $lösungenDir = Join-Path -Path $baseDir -ChildPath "Lösungen"
    $weitereDateienDir = Join-Path -Path $baseDir -ChildPath "Weitere Dateien"

    # Create folders if they don't exist
    if (!(Test-Path -Path $baseDir)) {
        New-Item -ItemType Directory -Path $baseDir | Out-Null
    }
    if (!(Test-Path -Path $aufträgeDir)) {
        New-Item -ItemType Directory -Path $aufträgeDir | Out-Null
    }
    if (!(Test-Path -Path $präsentationenDir)) {
        New-Item -ItemType Directory -Path $präsentationenDir | Out-Null
    }
    if (!(Test-Path -Path $lösungenDir)) {
        New-Item -ItemType Directory -Path $lösungenDir | Out-Null
    }
    if (!(Test-Path -Path $weitereDateienDir)) {
        New-Item -ItemType Directory -Path $weitereDateienDir | Out-Null
    }

    # Check the file extension and move it to the appropriate destination
    switch ($file.Extension) {
        '.docx' {
            if ($file.Name -like '*_L.docx') {
                Move-Item -Path $file.FullName -Destination $lösungenDir
                $destinationFolder = $lösungenDir
            } else {
                Move-Item -Path $file.FullName -Destination $aufträgeDir
                $destinationFolder = $aufträgeDir
            }
        }
        '.pptx' {
            Move-Item -Path $file.FullName -Destination $präsentationenDir
            $destinationFolder = $präsentationenDir
        }
        default {
            Move-Item -Path $file.FullName -Destination $weitereDateienDir
            $destinationFolder = $weitereDateienDir
        }
    }
```
Im schritt: ```#Folder names and variables for the next step``` werden die Paths erstellt und darunter die dazugehörigen Ordner, falls diese nicht vorhanden sind.

Danach wird die Datei geprüft auf die Endung, welches dann die Entscheidung beinflusst, in welchen Ordner die Datei kommt. Als Ordner gibt es die Optionen: Aufträge, Lösungen, Präsentationen und Weitere Dateien. Wie man oben sieht, kommen ```.docx``` Dateien in den *Aufträge* Ordner, wenn die .docx mit ```_L.docx``` aufhört, dann kommen diese in den *Lösungen* Ordner. Wenn die Datei ```.pptx``` heisst kommt diese in den *Präsentationen* Ordner und alles restliche kommt in den *Weitere Dateien* Ordner.


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
