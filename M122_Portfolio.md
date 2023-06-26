# Datei-Sortierungs Skript
In diesem Projekt geht es darum ein Skript zu erstellen, welches etwas auf dem Laptop automatisiert. Also habe ich mir gedacht, dass es sehr praktisch wäre ein Skript zu haben, welches mir automatisch die Dateien einsortiert, die ich von Moodle herunterlade. Dies bedeutet, dass das Skript die gewollten Dateien erkennt, in einen bestimmten Ordner sortiert und dies natürlich ohne Fehler.

## Inhalt
Wie gesagt habe ich in diesem Projekt einen Datei-Sortier Skript gemacht. Natürlich musste ich mich zuerst informieren, um überhaupt zu wissen, wie ich das mache. Das erste, welches ich gemacht habe, war GPT zu fragen, wie er es machen würde. Da habe ich gesehen, welche Befehle er benutzt. Um dies dann zu erweitern habe ich im Internet nachgeschaut wie dies geht.

Was mir sehr in diesem Projekt geholfen hat, ist das cmdlet ```Where-Object```, da ich den Download Ordner, als Ordner ausgewählt habe, von wo die Dateien genommen werden. Dadurch konnte ich einfach die einzelnen Dateien auswählen, die ich möchte und wenn ich weitere hinzufügen möchte, geht dies auch sehr einfach.

```ps
$files = Get-ChildItem -Path $downloadPath -File |Where-Object { $_.Name -like 'LA_*' -or $_.Name -like 'PR_*' -or $_.Name -like 'BL_*' -or $_.Name -like 'MLP_*' }
```
Dies ist der Code dafür und wenn ich, wie gesagt, etwas hinzufügen möchte, muss ich nur dies eingeben:

```ps
-or $_.Name -like '[neue Kondition]'
```

## Was habe ich in diesem Auftrag gelernt?
Ich habe gelernt, wie man Dateien, mit Powershell Befehlen, bewegt. Dies geht mit dem Behfel Move-Item. Ich habe dies so benutzt:
```ps
 Move-Item -Path [Dateiname] -Destination [Zielort]
```
### Finales Produkt:
```ps
<#
Scriptname: GoedertierTimo_Skript.ps1
Author: Timo Goedertier
Date: 26-06-2023
Version: 4.1
Description: Sortiert heruntergeladene Dateien von Moodle
#> 


# Der Zielordner, in dem die Modul Ordner(M-Ordner) drin sind, wird bestimmt
$destinationPath = "C:\Users\timog\OneDrive - BBBaden"
$downloadPath = "C:\Users\timog\Downloads"
$logFolder = "C:\Users\timog\OneDrive - BBBaden\M122\GoedertierTimo_LB_M122_2021-V3"

# Dateien werden bestimmt
# Das Where-Object wurde von GPT erstellt
$files = Get-ChildItem -Path $downloadPath -File | Where-Object { $_.Name -like 'LA_*' -or $_.Name -like 'PR_*' -or $_.Name -like 'BL_*' -or $_.Name -like 'MLP_*' }

# Für jede ausgewählte Datei wird dies gemacht
foreach ($file in $files) {
    try {
        # Extrahiere die Nummer nach LA_/PR_/BL_
        # Hier wurde mit GPT gearbeitet und mit der Hilfe von Herr A. Schmid
        [int]$number = ($file.BaseName -split '_')[1]
        # Überprüfe, ob die Nummer größer als 1000 ist
        if ($number -gt 1000) {
            # Wenn die Nummer größer ist, kommt sie in den IMS Lernattelier Ordner
            $baseDir = Join-Path -Path $destinationPath -ChildPath "IMS Lernattelier"
            $baseDir = Join-Path -Path $baseDir -ChildPath "M$number"
        }
        else {
            # Wenn nicht geht es weiter in den Modul Ordner
            $baseDir = Join-Path -Path $destinationPath -ChildPath "M$number"
        }
    }
    catch {
        # Protokolliere den Fehler und überspringe die Datei
        $logFile = Join-Path -Path $logFolder -ChildPath "fileMovement.log"
        $currentDate = Get-Date -Format "dd.MM.yyyy"
        $logMessage = "$currentDate - $($file.Name) failed to be processed and was skipped"
        Add-Content -Path $logFile -Value $logMessage
        continue
    }
    # Ordner Namen und Variabeln für den nächsten Schritt
    # Teilweise wurde dieser Abschnitt mit GPT gemacht
    $auftrageDir = Join-Path -Path $baseDir -ChildPath "Aufträge"
    $prasentationenDir = Join-Path -Path $baseDir -ChildPath "Präsentationen"
    $losungenDir = Join-Path -Path $baseDir -ChildPath "Lösungen"
    $weitereDateienDir = Join-Path -Path $baseDir -ChildPath "Weitere Dateien"

    # Erstelle die obigen Ordner, wenn sie nicht existieren
    if (!(Test-Path -Path $baseDir)) {
        New-Item -ItemType Directory -Path $baseDir | Out-Null
    }
    if (!(Test-Path -Path $auftrageDir)) {
        New-Item -ItemType Directory -Path $auftrageDir | Out-Null
    }
    if (!(Test-Path -Path $prasentationenDir)) {
        New-Item -ItemType Directory -Path $prasentationenDir | Out-Null
    }
    if (!(Test-Path -Path $losungenDir)) {
        New-Item -ItemType Directory -Path $losungenDir | Out-Null
    }
    if (!(Test-Path -Path $weitereDateienDir)) {
        New-Item -ItemType Directory -Path $weitereDateienDir | Out-Null
    }

    # Überprüfe die Endungen der Dateiei und sortiere diese nach den Kriterien ein
    switch ($file.Extension) {
        '.docx' {
            if ($file.Name -like '*_L.docx') {
                Move-Item -Path $file.FullName -Destination $losungenDir
                $destinationFolder = $losungenDir
            }
            else {
                Move-Item -Path $file.FullName -Destination $auftrageDir
                $destinationFolder = $auftrageDir
            }
        }
        '.pptx' {
            Move-Item -Path $file.FullName -Destination $prasentationenDir
            $destinationFolder = $prasentationenDir
        }
        '.xlsx' {
            if ($file.Name -like 'MLP_*') {
                Move-Item -Path $file.FullName -Destination $baseDir
                $destinationFolder = $baseDir
            }
            else {
                # Wenn die .xlsx-Datei nicht mit "MLP_" beginnt, führe die Standardaktion aus
                Move-Item -Path $file.FullName -Destination $weitereDateienDir
                $destinationFolder = $weitereDateienDir
            }
        }
        default {
            Move-Item -Path $file.FullName -Destination $weitereDateienDir
            $destinationFolder = $weitereDateienDir
        }
    }

    # Erstelle ein Protokoll für alle verschobenen Dateien
    $logFile = Join-Path -Path $logFolder -ChildPath "fileMovement.log"
    $currentDate = Get-Date -Format "dd.MM.yyyy"
    $logMessage = "$currentDate - $($file.Name) was moved to $destinationFolder"
    Add-Content -Path $logFile -Value $logMessage
}
```

## Was das Programm zeigt
Das Programm macht keine Ausgaben, nur eine Log Message für jede einsortierte Datei. Diese Log Message, kommt in die Datei fileMovement.log und kommt vor in diesem Style: ```DD.MM.YYYY - [filename] was moved to [foldername]```
Hier DD ist der Tag, MM, der Monat und YYYY das Jahr.

## Selbstreflexion
Ich hatte am Anfang ein Problem, da ich nicht wusste, was ich machen sollte. Als ich dann aber eine Idee hatte, ging alles schnell, da ich ein konkretes Ziel vor Augen hatte. Während dem Programmieren hat mit GPT auch viel geholfen, obwohl es am Anfang ein bisschen zu viel war. Während dem Arbeiten konnte ich mich auch gut konzentrieren und war sehr wenig abgelenkt, vorallem im Vergleich zu vorherigen Projekten. 

## Reflexion
### Was habe ich gut gemacht?

-Ich konnte mich gut konzentrieren während dem Arbeiten, weil ich ein konkretes Ziel vor Augen hatte.

-Ich habe meine Idee immer wieder ein bisschen erweitert, wenn ich fertig war, mit dem Teil, den ich mir vorgenommen hatte.

### Was habe ich weniger gut gemacht?

-Ich habe sehr lange überlegt, was ich machen soll und habe deswegen Zeit verloren.

-Am Anfang habe ich noch viel mit GPT gemacht, weil ich nicht wusste, wie ich anfangen soll.
