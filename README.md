 BitMatrix


 #BitMatrix v1.0.0 - Release

Dieses Tool wandelt BitLocker-Wiederherstellungsschlüssel (Recovery Keys) automatisch in scannbare Barcodes und QR-Codes um, um den IT-Support und die Systemadministration zu beschleunigen.

Ein PowerShell-Werkzeug, das den lokalen **BitLocker-Wiederherstellungsschlüssel** ausliest und ihn als **Code128-Barcode** (in 3 Blöcken) sowie als **QR-Code** anzeigt. Der Schlüssel kann anschließend mit einem Barcode-/QR-Scanner direkt in den BitLocker-Wiederherstellungsbildschirm eingelesen werden – ohne die 48 Ziffern von Hand abzutippen.

---

## Hintergrund / Zweck

Barcode-Scanner verhalten sich gegenüber dem System wie eine Tastatur und funktionieren bereits **vor dem Start von Windows**, also auch im BitLocker-Wiederherstellungsbildschirm. Damit lässt sich der lange Recovery-Key (48 Ziffern, 8 Gruppen à 6) per Scan in Sekunden eingeben, statt ihn fehleranfällig manuell zu tippen.

Dieses Werkzeug erzeugt den passenden Code aus dem Schlüssel der **aktuellen Maschine**.

---

## Funktionen

- Liest den Recovery-Key lokal aus (`Get-BitLockerVolume`, Fallback `manage-bde`)
- Zeigt zusätzlich die **Key-Protector-ID** an → Abgleich mit der ID auf dem Wiederherstellungsbildschirm
- Erzeugt **Code128-Barcode in 3 Blöcken** (selbst gerendert, keine Schriftart nötig)
- Erzeugt **QR-Code** – vollständig vom Skript selbst generiert (eigener QR-Encoder, keine externe Software nötig)
- Ein gemeinsames PNG, geöffnet im lokalen Bildbetrachter **IrfanView**
- **Komplett offline** – keine Module-Installation, kein Internet zur Laufzeit nötig
- PNG wird nach dem Schließen standardmäßig wieder gelöscht (enthält den Klarschlüssel)

---

## Voraussetzungen

| Komponente | Hinweis |
|---|---|
| Windows mit BitLocker | Laufwerk muss verschlüsselt sein |
| Ausführung als **Administrator** | Pflicht zum Auslesen des Keys |
| Windows PowerShell 5.1 | Standard unter Windows 10/11 |
| **IrfanView** | zur Anzeige des PNG |

---

## Nutzung

```powershell
# PowerShell als Administrator öffnen, dann:
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\BitMatrix.ps1
```



## Wichtige Hinweise

- **Verifizieren vor dem Ernstfall:** Notepad öffnen, Codes hineinscannen und mit dem angezeigten Key vergleichen. Der Piepton bedeutet nur „erfolgreich gelesen", nicht „korrekt".
- **Scanner-Suffix (Enter/CR) deaktivieren**, sonst wird der Wiederherstellungsbildschirm zwischen den Blöcken evtl. vorzeitig bestätigt.
- **Mehrere Keys möglich:** Bei mehreren Recovery-Keys die angezeigte ID mit der Key-ID auf dem Wiederherstellungsbildschirm abgleichen.
- Das erzeugte PNG enthält den **Klarschlüssel** – sorgsam behandeln.

---

## Stand der Umsetzung (was wurde gemacht)

- [x] Auslesen des Recovery-Keys inkl. Key-Protector-ID
- [x] Code128-Barcode in 3 Blöcken (funktioniert zuverlässig, getestet mit Zebra DS2278)
- [x] QR-Code wird direkt vom Skript erzeugt und ist scanbar
- [x] Anzeige im PNG über IrfanView
- [x] Vollständig offline lauffähig

## Geplant / To-do (was noch zu tun ist)

- [x] **Bereitstellung als `.exe`**, damit das Tool ohne PowerShell-Kenntnisse per Doppelklick läuft
      (z. B. mit dem Modul `ps2exe`: `Invoke-ps2exe .\BitMatrix.ps1 .\Show-BitLockerBarcode.exe`)
  
---

## Dateien

| Datei | Beschreibung |
|---|---|
| `BitMatrix.ps1` | Hauptskript |
| `README.md` | Diese Dokumentation |

---

## Haftungsausschluss

Internes Hilfswerkzeug für den IT-Support. Nutzung auf eigene Verantwortung. Der Wiederherstellungsschlüssel ist sicherheitskritisch – erzeugte Bilder bitte nicht ungeschützt speichern oder weitergeben.
