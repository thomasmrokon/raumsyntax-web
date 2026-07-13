# raumsyntax-web

Statische Dachmarken-Website für [raumsyntax.de](https://raumsyntax.de) — Hub-Seite mit Produktübersicht (AID, DNA), Impressum, Datenschutz.

## Deployment

Kein GitHub Pages. Die Seite wird zusammen mit der [AID](https://github.com/thomasmrokon/AID)-App auf einem einzelnen selbstverwalteten Server (netcup VPS) via nginx ausgeliefert:

```
raumsyntax.de/       → statische Dateien aus diesem Repo (nginx root)
raumsyntax.de/aid/   → AID-Streamlit-App (nginx proxy_pass → 127.0.0.1:8501)
```

Deploy-Skript: `AID/deploy.sh` (dort auch für dieses Repo angepasst — clont beide Repos auf den Server).

## Struktur

- `index.html` — Hub-Seite (Produktkarten AID/DNA)
- `impressum.html`
- `datenschutz.html`
