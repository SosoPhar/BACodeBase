# BACodeBase

# Delphi – SFBT-Coach (Gradio + OpenAI)

Dieses Projekt enthält eine experimentelle Chatbot-Anwendung, die auf der **Solution-Focused Brief Therapy (SFBT)** basiert.
Die Anwendung wird mit **Gradio** lokal im Browser ausgeführt und verwendet die **OpenAI Chat Completions API**.

---

## Funktionen

* Strukturierte Gesprächsführung nach dem 9-Schritte-Flow der SFBT
* Pro Antwort wird genau **eine Frage** generiert
* Kurze, prägnante Antworten (1–3 Sätze)
* Wort-für-Wort-Streaming mit dynamischer Denk- bzw. Wartezeit
* Logging aller Sitzungen in einer Logdatei (`log6.log`)
* Impressum- und Datenschutzhinweise direkt im UI verfügbar

---

## Voraussetzungen

* **Python** Version 3.10 oder neuer
* Ein gültiger **OpenAI API-Key** mit Zugriff auf das verwendete Modell

### Notwendige Pakete

Die folgenden Python-Pakete müssen installiert sein:

```bash
pip install gradio openai
```

(`time`, `math`, `re`, `datetime` gehören zur Standardbibliothek von Python.)

> Für eine saubere Installation empfiehlt es sich, eine **virtuelle Umgebung** zu verwenden (`python -m venv .venv`).

---

## API-Key einrichten

Im Code befindet sich ein Platzhalter:

```python
client = openai.OpenAI(api_key="putyourkeyhere")
```

Ersetzen Sie `"putyourkeyhere"` durch Ihren **OpenAI API-Key** (Format: `sk-...`).

> 🔐 Hinweis: Der API-Key sollte **nicht dauerhaft** im Code hinterlegt werden. Für produktiven Einsatz empfiehlt es sich, den Key über eine Umgebungsvariable einzulesen.

---

## Ausführung

1. Speichern Sie den Code z. B. als `app.py`.
2. Führen Sie ihn aus mit:

```bash
python app.py
```

3. Nach dem Start zeigt die Konsole eine lokale URL (z. B. `http://127.0.0.1:7860`), die Sie im Browser öffnen können.
   Da im Skript `share=True` gesetzt ist, erzeugt Gradio zusätzlich einen temporären **öffentlichen Link**.

---

## Nutzung

* Schreiben Sie Ihre Nachricht in das Texteingabefeld.
* Der Chatbot antwortet **Wort für Wort**.
* Vor der Antwort wartet der Chatbot eine **dynamische Denkzeit**, die mit einer Heuristik (Flesch-Reading-Ease) berechnet wird.
* Die Gesprächsführung orientiert sich am **systemischen SFBT-Ansatz** und folgt einem klar definierten **9-Schritte-Flow**.

---

## Konfigurierbare Parameter

Im Skript lassen sich u. a. folgende Einstellungen verändern:

```python
COACH_WORD_DELAY = 0.15       # Verzögerung zwischen gestreamten Wörtern (Sekunden)
BASE_TIP_ANIMATION_TIME = 4   # Basis-Denkzeit (Sekunden)
LOGFILE = "log6.log"          # Speicherort der Logdatei
```

Die **gesamte Wartezeit** wird berechnet als:

```
total_thinking_time = 4 + calculate_delay(reply)
```

---

## Logging

Alle Interaktionen werden in die Datei **`log6.log`** geschrieben, darunter:

* Start und Ende jeder Antwort
* Geplante und tatsächliche Wartezeit
* Streaming-Dauer
* Gesamt-Processing-Zeit pro Antwort

Die Logdatei befindet sich im aktuellen Arbeitsverzeichnis.

---

## Modelle & Kosten

Im Skript ist aktuell das Modell **`gpt-4-1106-preview`** eingestellt:

```python
response = client.chat.completions.create(
    model="gpt-4-1106-preview",
    ...
)
```

Falls dieses Modell nicht verfügbar ist, können Sie es z. B. durch **`gpt-4o`** oder **`gpt-4o-mini`** ersetzen. Bitte beachten Sie die jeweiligen **Token-Kosten** des gewählten Modells.

---

## Fehlerbehebung

* **401 Unauthorized / Invalid Authentication**
  → API-Key prüfen (korrekt eingefügt? gültig? Zugriff auf Modell vorhanden?)

* **Modell nicht verfügbar („model not found“)**
  → Modellnamen anpassen oder sicherstellen, dass Ihr Account Zugriff hat.

* **Verbindungsprobleme**
  → Internetverbindung prüfen; ggf. Gradio oder OpenAI-Paket aktualisieren.

---

## Datenschutz

Die App enthält einen eingebauten Abschnitt **Impressum & Datenschutz**.
Es werden **keine personenbezogenen Daten gespeichert**.
Gesendete Texte werden ausschließlich zur Verarbeitung durch die **OpenAI API** und **Gradio** genutzt.

Die Antworten des Chatbots sind **automatisch generiert** und stellen **keine professionelle Beratung** dar.

---

## Lizenz / Nutzungshinweis

Dieses Projekt ist Teil einer **Bachelorarbeit** und dient ausschließlich zu **wissenschaftlichen Forschungszwecken**.
Es handelt sich um ein nicht-kommerzielles, prototypisches System.

