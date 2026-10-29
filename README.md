# FCE DRIVER MARKET – V2.3-FCE1

Angepasste Version des bewährten WTV Driver Market V2.3 für **FCE – Formula Central Europe**.

- FCE-Branding in Blau und Orange statt WTV-Farben.
- 11 FCE-Teams statt der zwei alten WTV-Teamrollen.
- Team-Rollen-IDs stehen übersichtlich in `team_roles.json` (zunächst `0`, **bitte unbedingt anpassen**).
- Bewährte Fahrer-, Vertrags-, Marktwert-, Transfer-, Saison-, Audit- und Backup-Funktionen bleiben erhalten.
- Discord-Admin-Berechtigung für die Verwaltungsbefehle (wie im Original V2.3).

## Slash-Commands (13)

**Öffentlich:** `/drivermarket`, `/alldrivers`, `/driverinfo`, `/help`, `/version`, `/seasonstatus`

**Discord-Administratoren:** `/saison`, `/adddriver`, `/editdriver`, `/transferdriver`, `/removedriver`, `/datastatus`, `/auditlog`

Beispiel:
`/adddriver fahrer:<Discord-Nutzer> marktwert:25 vertrag:3 ausstiegsklausel:50 gehalt:8 boni:1 Mio. pro Sieg`

## Dateien

- `bot.py` – Bot-Programm
- `team_roles.json` – Zuordnung der FCE-Teams zu ihren Discord-Rollen
- `requirements.txt` – Python-Abhängigkeiten
- `.env.example` – Vorlage für Token & Server-ID (echte Tokens nur in Railway-Variables oder lokaler `.env`)
- `RAILWAY_ANLEITUNG.md` – vollständige Schritt-für-Schritt-Anleitung

**Start auf Railway:** `python bot.py`  
**Daten:** `/data/driver_market.json` (Railway-Volume muss vorher eingebunden werden)

**Wichtig:** Dies ist ein neuer, separater FCE-Bot. Du musst die FCE-Teamrollen und FCE-Server-ID noch eintragen; echte Discord-/Railway-Verbindungen konnten hier nicht getestet werden. Bitte nicht den bestehenden WTV-Service oder dessen Volume überschreiben.
