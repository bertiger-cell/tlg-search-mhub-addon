# TLG Search (Telegram) — mHub Addon v2

Statisches Addon: drei JSON-Dateien, kein Server, kein Build. Hochladen auf jeden
statischen Webhost (Netlify, Cloudflare Pages, GitHub Pages, nginx, S3 …).

## Dateien

```
mhub-addon.json                     Manifest
catalog/video/search-themes.json    Kacheln: Filme, Serien, Musik, Software, E-Books
catalog/video/bot.json              Kacheln: Bot starten, Inline-Suche
```

Die Quellen stehen inline in den Katalog-Items (`Item.sources`), deshalb gibt es
keine `source`-Dateien. Das Protokoll sieht das ausdrücklich vor, und statisch ist
es die einzige portable Variante: ein `/source`-Pfad müsste als Datei `tg:filme.json`
heißen, und `:` ist auf Windows kein gültiger Dateiname.

## Installation

1. Den Ordner `tlg-search/` hochladen, so dass `mhub-addon.json` direkt unter einem
   Verzeichnis liegt, z. B. `https://example.com/tlg-search/mhub-addon.json`.
2. In mHub als Addon eintragen:

```
https://example.com/tlg-search/
```

## Was das Addon tut

`@tlg_searchbot` ist ein Telegram-Bot für Dateisuche und -auslieferung (Filme,
Serien, Musik, Software, E-Books). Eine öffentliche API, die ein statisches Addon
abfragen könnte, gibt es dafür nicht. Das Addon ist deshalb ein Launcher: die
Kacheln tragen den Deep-Link in den Bot, mHub öffnet ihn in Telegram, gesucht und
angefordert wird dort.

Alle Quellen tragen `kind: "website"` — der Deep-Link ist keine abspielbare
Stream-URL, und genau dafür ist dieser Wert in der Spezifikation gedacht. Es gibt
deshalb bewusst kein `resolve`, keine erfundenen Stream-URLs und keine erfundenen
Metadaten (Genres, Jahr, Cover).

`triggers` meldet das Addon zusätzlich an, sobald ein `t.me/tlg_searchbot`-Link
im Browser aufgerufen wird.

## Start-Code

Alle Links enthalten `?start=_tgr_-3d8H2U4Nzky`. Nach dem eigenen Start-Code von
Telegram diesen Wert in den drei JSON-Dateien ersetzen (Stichwort `t.me/tlg_searchbot`).

## Erweitern (optional)

Echte Suchergebnisse als Katalog-Items brauchen eine Backend-Seite: resolve und
client-fetch als Skript, das die Telegram-Instanz des Nutzers abfragt. Statisch
bleibt der Bot die Quelle der Wahrheit, mHub der Einstieg.