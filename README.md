# raumsyntax-web

Statische Dachmarken-Website für [raumsyntax.de](https://raumsyntax.de) — Hub-Seite mit Produktübersicht (AID, DNA), Impressum, Datenschutz.

## Deployment

Kein GitHub Pages. Die Seite wird zusammen mit der [AID](https://github.com/thomasmrokon/AID)-App auf einem einzelnen selbstverwalteten Server (netcup VPS) via nginx ausgeliefert:

```
raumsyntax.de/       → statische Dateien aus diesem Repo (nginx root)
raumsyntax.de/apps/  → Auswahlseite der Prototypen, nginx Basic Auth
raumsyntax.de/aid/   → AID-Streamlit-App (nginx proxy_pass → 127.0.0.1:8501)
```

Auf dem Server liegt unter `/opt/raumsyntax-web` ein Klon dieses Repos, der
dem Deploy-Nutzer gehört und direkt der nginx-`root` ist. Ein Push nach
`origin/main` allein ändert an der Live-Seite deshalb **nichts** — es fehlt
der Pull auf dem Server:

```
git push
ssh deploy@37.221.198.106 'git -C /opt/raumsyntax-web pull'
```

Prüfen:

```
curl -sI https://raumsyntax.de/ | head -1
```

Erstinstallation und AID-App: `AID/deploy.sh` — legt den Klon an, richtet
Dienst und vhost ein. Eine bereits vorhandene vhost-Datei wird dabei nicht
überschrieben (certbot verwaltet dort `listen`-Ports und TLS-Pfade); die
Vorlage landet als `.new` daneben.

## Struktur

- `index.html` — Hub-Seite (Produktkarten AID/DNA)
- `apps/index.html` — Auswahlseite mit Link in jede App; liegt hinter Basic Auth
- `impressum.html`
- `datenschutz.html`

## Zugang zur Auswahlseite

`/apps/` ist die einzige geschützte Stelle: davor steht nginx Basic Auth
(`/etc/nginx/.htpasswd-apps`), dahinter bringt jede App ihren eigenen Login
mit. Weiteren Zugang anlegen:

```
ssh deploy@37.221.198.106
printf '%s:%s\n' NAME "$(openssl passwd -apr1)" | sudo tee -a /etc/nginx/.htpasswd-apps
```
