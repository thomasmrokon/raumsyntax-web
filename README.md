# raumsyntax-web

Statische Website für raumsyntax.de: `index.html`, `impressum.html`, `datenschutz.html`.
Keine Skripte, keine Cookies, keine externen Ressourcen.

Die Startseite stellt die Beratungstätigkeit vor (Beratung, Prozessanalyse, Workshops, Keynotes).
Forschung, Lehre und Vorträge stehen auf mrokon.de; beide Seiten verlinken sich gegenseitig.

## Deployment

Die Seite wird per nginx von einem selbstverwalteten Server ausgeliefert. Auf dem Server liegt unter
`/opt/raumsyntax-web` ein Klon dieses Repos, der direkt der nginx-`root` ist. Ein Push nach
`origin/main` allein ändert an der Live-Seite nichts – es fehlt der Pull auf dem Server:

```
git push
ssh deploy@37.221.198.106 'git -C /opt/raumsyntax-web pull'
```

Prüfen:

```
curl -sI https://raumsyntax.de/ | head -1
```
