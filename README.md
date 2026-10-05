# cd-sale

Fotos (Vorder-/Rückseite) → `items.csv` mit Metadaten → statische Verkaufsseite (GitHub Pages), deutsch.

## Einrichten (einmal)
    cp .env.example .env     # ANTHROPIC_API_KEY eintragen (für Bildanalyse, Beschreibungen, Genres); MusicBrainz braucht keinen Key
    ./run.sh                 # legt beim ersten Mal die Python-Umgebung an

## Ablauf
1. Neue Fotos in `photos/` legen: pro CD zuerst die Vorderseite, dann die Rückseite (nach Dateiname sortiert).
2. `./run.sh`: liest Barcodes, holt Interpret/Album/Titelliste bei MusicBrainz, sonst per Bildanalyse.
   - Fotos werden nummeriert und umbenannt (`001front.jpg`, `001back.jpg`); die alten Namen stehen in `renamed.log`.
   - Fehlt eine Seite, entsteht der Eintrag trotzdem mit Bemerkung. Nachliefern: Datei als `028back.jpg` ablegen.
   - Danach `items.csv` prüfen (Spalte `check`), von Hand korrigieren und `./run.sh` erneut starten.
3. Die Seite entsteht in `docs/` (Grossansichten 1800 px, Vorschau 480 px), dazu `liste.txt`.
4. Veröffentlichen: `git add -A && git commit -m "Update" && git push` (GitHub Pages: Branch `main`, Ordner `/docs`).

## Spalten und Optionen
- `artist` (Künstler) und `title` getrennt; Standardsortierung nach Künstler, Artikel wie "Die"/"The" werden ignoriert. `sort_title` überschreibt die Sortierung des Titels.
- `genre`: englische Namen, mehrere mit Komma (z. B. `Pop, Rock`); Liste in `sale.py` (GENRES["cd"]), `Brass Band` = Blasmusik.
- `rare` = Sammlerstück-Markierung, `remarks_de` = Bemerkung.
- `config.json`: `languages: ["de"]` (nur deutsch), `show_language: false` (Spalte Sprache ausblenden).
