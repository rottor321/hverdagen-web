# hverdagen-web

Nettsiden til iPhone-appen **Hverdagen** — <https://rott.no/hverdagen/>.

Ren HTML og CSS, uten byggesteg. Fargene er de samme som i appen
(`Huset/DesignSystem/Theme.swift`), med lys og mørk modus.

| Side | Adresse |
|---|---|
| Forside | `/hverdagen/` |
| Personvernerklæring | `/hverdagen/personvern/` |
| Support | `/hverdagen/support/` |

Personvern- og support-URL-ene er de som oppgis i App Store Connect. Endres
appens datahåndtering, skal `personvern/index.html` oppdateres samtidig — og
App Privacy-svarene i App Store Connect med den.

## Utrulling

Plesk på `rott.no` henter dette repoet med Git-utvidelsen og legger det i
`httpsdocs/hverdagen`. **Ikke `httpdocs`:** domenet har en gammel separat
SSL-dokumentrot fra 2006, så HTTPS serveres fra `httpsdocs`. En push til `main`
deployes automatisk via webhook (GitHub → Plesk, levert som 204 når det virker).

`http://rott.no/hverdagen/…` videresendes med 301 til `https` av en `.htaccess`
som ligger **utenfor dette repoet**, i `httpdocs/hverdagen/` på serveren:

```
RewriteEngine On
RewriteCond %{HTTPS} !=on
RewriteRule ^(.*)$ https://rott.no/hverdagen/$1 [R=301,L]
```

Den ligger der fordi leverandøren har låst Plesks egen http→https-bryter for
domenet. Den er bevisst begrenset til `/hverdagen`: Let's Encrypt fornyer
sertifikatet over http fra `httpdocs`, og en videresending i roten kunne
stoppet fornyelsen.
