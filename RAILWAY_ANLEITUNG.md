# FCE Driver Market – Railway-Anleitung

## 1. GitHub-Repository vorbereiten

1. Auf https://github.com ein **privates** Repository `FCE-DriverMarket` erstellen.
2. ZIP entpacken und **die Dateien aus dem Ordner** direkt ins Hauptverzeichnis des Repositorys hochladen: `bot.py`, `requirements.txt`, `team_roles.json`, `.python-version`, `.env.example`, `README.md`, `RAILWAY_ANLEITUNG.md`, `.gitignore`.
3. **Keine** `.env`, `driver_market.json` oder echten Bot-Tokens hochladen.
4. In `team_roles.json` die Zahl `0` bei jedem Team durch die **Discord-Rollen-ID aus deinem FCE-Server** ersetzen. Die Namen können nach Bedarf angepasst werden. Auf GitHub: Datei öffnen, Bearbeiten (Stiftsymbol), speichern/committen. Die 11 IDs müssen aus deinem *eigenen FCE-Server* stammen, nicht vom WTV-Server.
5. Wichtig: Du musst nicht gleich alle Rollen eintragen, aber ohne zugeordnete Rollen erkennt `/adddriver` den Teamnamen nicht automatisch und `/transferdriver` zu diesem Team ist gesperrt. Das verhindert fehlerhafte Zuordnungen.

## 2. Eigenen Bot in Discord einrichten

1. https://discord.com/developers/applications öffnen und für **FCE** eine eigene Discord-App / Bot anlegen (empfohlen, falls dein bisheriger Bot bei WTV weiterläuft).
2. Token im Developer Portal generieren/kopieren und **geheim halten**.
3. Unter **Bot → Privileged Gateway Intents** den **Server Members Intent** einschalten.
4. Unter **OAuth2 → URL Generator** die Scopes `bot` und `applications.commands` sowie die Berechtigung **Rollen verwalten** auswählen. Den generierten Link öffnen und die App auf den FCE-Server einladen. Empfohlen zusätzlich: `Kanäle ansehen`, `Nachrichten senden`, `Links einbetten`, `Nachrichtenverlauf lesen`.
5. In den Discord-Servereinstellungen die Rolle des Market-Bots **über alle 11 Teamrollen** verschieben, die er verwalten soll. `Administrator` braucht der **Bot selbst** nicht. Die **Verwaltungs-Slash-Commands** dieser V2.3 sind allerdings nur für Personen mit Discord-Administratorberechtigung freigeschaltet.

## 3. Railway-Service erstellen

1. https://railway.com/dashboard öffnen. Du kannst **im selben Railway-Projekt** wie der Welcome Bot einen **neuen Service** hinzufügen, aber verwende dafür ein eigenes GitHub-Repository und ein eigenes Volume.
2. **+ New → GitHub Repo** (je nach Oberfläche: „Deploy from GitHub Repo“) → `FCE-DriverMarket` auswählen.
3. Beim neuen Service auf **Variables** klicken und folgende Variablen als einzelne Werte speichern:

   `DISCORD_TOKEN` = der **FCE-Driver-Market-Bot-Token**

   `GUILD_ID` = die ID deines FCE-Discord-Servers

4. Unter **Settings → Deploy → Custom Start Command** eintragen:

   `python bot.py`

5. **Ganz wichtig: Volume erstellen und mit diesem Service verbinden**, Mount Path **`/data`**. Railway setzt `RAILWAY_VOLUME_MOUNT_PATH` automatisch. **Nicht selbst manuell setzen.** Der Bot speichert dann unter `/data/driver_market.json` sowie `/data/driver_market.backup.json`.
6. Änderungen deployen / `Redeploy` starten. Eine öffentliche Domain ist für diesen Bot nicht nötig.

## 4. Funktion prüfen

- In Railway unter **Deploy Logs** auf Start und Fehlermeldungen achten.
- Auf dem FCE-Server `/version`, `/drivermarket` und als Discord-Admin `/datastatus` ausführen.
- Mindestens einem Testfahrer die Teamrolle geben und dann `/adddriver` testen.
- Testweise `/transferdriver` zwischen zwei Teams prüfen – das sollte die Teamrolle auf Discord ebenfalls ändern.
- Immer nur **eine aktive Instanz** desselben Bots betreiben, damit keine Doppelaktionen entstehen.

## 5. Datensicherheit & Updates

- **Nie dasselbe Railway-Volume wie beim WTV-Bot nutzen**, sonst könnten Fahrer-/Vertragsdaten überschrieben werden. Getrennte Services und getrennte Volumes sind wichtig.
- Auch beim Wechsel vom Welcome Bot zum Driver Market werden **getrennte** Volumes verwendet; beide Bots laufen unabhängig.
- Jeder Commit bei GitHub kann Railway ein neues Deployment auslösen. Die gespeicherten Daten bleiben mit dem korrekt verbundenen Volume erhalten.
- `driver_market.backup.json` ist ein Backup im **gleichen Volume**, kein externer Schutz. Erstelle bei wichtigen Saisonständen zusätzliche Railway-Volume-Backups bzw. lade die Datei mit der Railway CLI herunter.
- Bereits vorhandene FCE-Daten nur nach bewusster Sicherung importieren. **WTV-Daten niemals blind als FCE-Daten verwenden.**
- Wenn der Bot beim Start `PrivilegedIntentsRequired` meldet: Server Members Intent einschalten. Bei `Missing Permissions`: Rollenverwaltung + Hierarchie prüfen. Wenn 0 Teamrollen erkannt werden: `team_roles.json` mit echten IDs ausfüllen und redeployen.

## 6. Wichtig bei mehreren Ligen

Die V2.3 ist ein gemeinsamer Fahrermarkt **pro Discord-Server**. FCE Liga 1, Liga 2 und Liga 3 nutzen darin denselben Datenbestand. Separate Fahrermärkte pro Ligaklasse wären eine spätere Erweiterung und sind hier nicht enthalten.
