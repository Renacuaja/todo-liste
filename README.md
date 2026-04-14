# Einfache ToDo-Liste für den Alltag

Eine einfache, aber funktionsreiche Web-App für gemeinsame Aufgabenlisten — nutzbar von mehreren Menschen gleichzeitig, mit Spracheingabe, KI-Korrektur und einem Admin-System.

Diese App wurde von Eva Renacuaja gemeinsam mit [Claude](https://claude.ai) (KI-Assistent von Anthropic) entwickelt und gebaut.

---

## Was die App kann

- **Aufgaben eingeben** — per Texteingabe oder Spracheingabe
- **KI-Korrektur** — gesprochene Aufgaben werden automatisch auf Rechtschreibung, Großschreibung von Nomen und Satzzeichen korrigiert
- **Codewörter** — besondere Schlüsselwörter am Satzanfang steuern das Verhalten:
  - `Morgen Aufgabe` → Aufgabe wird direkt in den nächsten Tag eingetragen
  - `Täglich Aufgabe` → Aufgabe wiederholt sich täglich
  - `Zweitägig Aufgabe` → Aufgabe wiederholt sich alle 2 Tage
  - `Wöchentlich Aufgabe` → Aufgabe wiederholt sich wöchentlich
  - `Montags Aufgabe` (und andere Wochentage) → Aufgabe wiederholt sich an diesem Wochentag
  - `Am 22.4. Aufgabe` → Aufgabe wird direkt in das angegebene Datum eingetragen
- **Datumsnavigation** — mit Pfeilen durch vergangene und zukünftige Tage blättern
- **Teilnehmer** — Aufgaben können einzelnen Personen zugewiesen werden
- **Mehrere Listen** — beliebig viele ToDo-Listen, jede mit eigenem Namen und Losungswort
- **Admin-System** — der erste Nutzer einer Liste wird automatisch Admin und kann das Losungswort ändern
- **Master-Admin** — Übersicht aller Listen mit der Möglichkeit, einzelne Listen zu löschen

---

## Technische Voraussetzungen

- Ein kostenloses [Firebase](https://firebase.google.com)-Konto mit einer Realtime Database.
- Ein [GitHub](https://github.com)-Konto mit aktivierten GitHub Pages.
- Ein [Anthropic](https://anthropic.com)-API-Schlüssel für die KI-Korrektur der Spracheingabe.

---

## Einrichtung

### 1. Firebase anlegen

1. Gehe zu [firebase.google.com](https://firebase.google.com) und erstelle ein neues Projekt
2. Aktiviere die **Realtime Database** (im kostenlosen Spark-Tarif)
3. Notiere die Datenbank-URL (sieht so aus: `https://dein-projekt-default-rtdb.europe-west1.firebasedatabase.app`)

### 2. Firebase-Regeln setzen

Gehe in der Firebase-Konsole zu **Realtime Database → Regeln** und ersetze den Inhalt mit folgendem:

```json
{
  "rules": {
    "todos": {
      "config": {
        ".read": true,
        ".write": true
      }
    },
    "listen": {
      ".read": true,
      ".write": true
    },
    "master": {
      ".read": true,
      ".write": true
    },
    "tage": {
      ".read": true,
      ".write": true
    },
    "wiederkehrend": {
      ".read": true,
      ".write": true
    },
    "teilnehmer": {
      ".read": true,
      ".write": true
    }
  }
}
```

### 3. Das erste Losungswort setzen

1. Gehe in der Firebase-Konsole zu **Realtime Database → Daten**
2. Erstelle folgende Struktur manuell:
   - `todos` → `config` → `passwort` → `"DeinLosungswort"`

### 4. Die index.html anpassen

Öffne die Datei und suche folgende Zeile am Anfang des JavaScript-Bereichs:

```javascript
const DB_ROOT = 'https://todo-liste-d024d-default-rtdb.europe-west1.firebasedatabase.app';
```

Ersetze die URL durch deine eigene Firebase-Datenbank-URL.

Suche außerdem nach der Zeile mit dem Anthropic-API-Aufruf:

```javascript
const response = await fetch('https://api.anthropic.com/v1/messages', {
```

Die KI-Korrektur funktioniert nur, wenn dein Anthropic-API-Schlüssel in einem eigenen Backend oder Proxy eingebunden ist — da API-Schlüssel nicht direkt im Frontend-Code stehen sollten. Alternativ kann die KI-Korrektur einfach entfernt werden, ohne dass die restliche App beeinträchtigt wird.

### 5. Auf GitHub Pages veröffentlichen

1. Erstelle ein neues Repository auf GitHub
2. Lade die `index.html` hoch
3. Gehe zu **Settings → Pages** und aktiviere GitHub Pages für den `main`-Branch
4. Die App ist dann unter `https://dein-nutzername.github.io/dein-repo/` erreichbar

---

## Erste Schritte nach der Einrichtung

1. Öffne die App im Browser
2. Gib einen **Listen-Namen** und dein **Losungswort** ein
3. Der erste Nutzer wird automatisch zum Admin der Liste
4. Das kleine **⚙-Symbol** ganz unten auf dem Login-Bildschirm öffnet den Master-Admin-Bereich

---

## Mitmachen und Weiterverwenden

Du bist herzlich eingeladen, diese App zu kopieren, für dich anzupassen und weiterzuverwenden! Wenn du Verbesserungen einbaust, freuen wir uns, davon zu hören — erstelle einfach ein **Issue** in diesem Repository.

Entwickelt mit ❤️ und [Claude](https://claude.ai) von Anthropic.
