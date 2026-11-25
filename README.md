# Wunschzettel

Eine Wunschliste als Webseite, gedacht für Weihnachten. Jeder Wunsch hängt als Geschenkanhänger an einer Kordel. Wer den Link hat, kann Geschenke reservieren oder für teure Wünsche zusammenlegen. Der Besitzer der Liste sieht davon nichts.

Die Seite zeigt ohne passenden Link nur einen Hinweis. Mit `?demo=1` am Ende der Adresse startet sie mit Platzhalter-Wünschen im Demo-Modus.

## Funktionen

**Für Besucher**
- Wünsche mit Produktbild, Beschreibung, Preis, Größe, Shop-Link und Wichtigkeit (★ bis ★★★)
- „Ich besorge das“: reserviert einen Wunsch, ein Stempel markiert ihn als vergeben. Zurücknehmen kann nur, wer reserviert hat
- Zusammenlegen: Jeder trägt seinen Betrag ein, ein Balken zeigt den Stand
- Losmaschine: zieht per Animation einen zufälligen freien Wunsch
- Filter nach Preis und „Noch frei“, Sortierung nach Wichtigkeit oder Preis
- Countdown bis Heiligabend, abschaltbarer Schnee, ein verstecktes Easter Egg

**Für den Besitzer**
- Anmeldung mit Google, danach Bearbeitungsansicht
- Wünsche anlegen, bearbeiten, löschen
- Bilder hochladen. Hochgeladene Bilder verkleinert der Browser auf höchstens 900 px
- „Link teilen“ verschickt den Link über das Teilen-Menü am Handy oder kopiert ihn
- Vorschau der Besucheransicht ohne Reservierungen

## Technik

- Eine einzige Datei: `index.html` mit HTML, CSS und JavaScript, ohne Build-Schritt
- Hosting: GitHub Pages
- Datenbank: Firebase Cloud Firestore
- Anmeldung: Firebase Authentication. Besucher werden unsichtbar anonym angemeldet, der Besitzer per Google-Pop-up
- Firebase JS SDK 13.2.0, zur Laufzeit vom CDN geladen
- Schriften: Gloock und Atkinson Hyperlegible (Google Fonts)

## Datenschutz und Sicherheit

- **Keine Wünsche im Repository.** Hier liegt nur der Code. Wünsche, Reservierungen und Beiträge liegen in Firestore.
- **Der Link ist der Schlüssel.** Jede Liste hat eine zufällige 20-stellige ID (`?l=…`). Die Sicherheitsregeln erlauben, eine Liste mit bekannter ID zu lesen, verbieten aber, Listen aufzuzählen.
- **Nur der Besitzer schreibt Wünsche.** Eine Liste anlegen darf nur die Google-Adresse, die in den Regeln steht.
- **Spoiler-Schutz:** Die Regeln sperren Reservierungen und Beiträge für den angemeldeten Besitzer. Öffnet er den Link ohne Anmeldung, sieht er sie trotzdem.
- `noindex`: Suchmaschinen nehmen die Seite nicht auf.

## Datenmodell (Firestore)

```
listen/{listenId}                     besitzer, erstellt
listen/{listenId}/wuensche/{id}       titel, notiz, preis, prio, groesse, link, bild, zusammen, geaendert
listen/{listenId}/reservierungen/{id} name, uid, zeit        (ID = Wunsch-ID, eine pro Wunsch)
listen/{listenId}/beitraege/{id}      wunsch, name, betrag, uid, zeit
```