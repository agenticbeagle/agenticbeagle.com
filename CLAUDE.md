# agenticbeagle.com

De website van AgenticBeagle. Hij staat op GitHub Pages: de branch `main`, map `/docs`, met het eigen domein agenticbeagle.com (`CNAME`) en HTTPS. Er werken twee mensen aan, elk op een eigen Mac met een eigen GitHub-account.

## Bestanden

Alleen wat in `docs/` staat, komt op de website. Dit bestand en andere werkbestanden staan erbuiten en blijven daar. De repo is wel openbaar: alles wat erin staat, ook buiten `docs/`, is op GitHub te lezen. Zet dus nergens in de repo namen van mensen of accounts, privégegevens of iets wat niet openbaar mag.

- `docs/index.html`: de voorpagina. Alles zit in dit ene bestand: de CSS, de afbeeldingen en de drie lettertypes (Newsreader, Inter en JetBrains Mono; SIL Open Font License) staan er als base64 in. Er is geen JavaScript.
- `docs/privacy.html`: de privacypagina, in dezelfde stijl en met een eigen kopie van de lettertypes. Onderaan de voorpagina staat de link "Privacy".
- `docs/kaartjes/`: de pagina's van de app Kaartjes, in de stijl van de app zelf (de kleuren van de app, de systeemletter van de iPhone, licht en donker) en niet in die van de voorpagina. Net als de rest van de site staat alles in de pagina zelf (CSS en afbeeldingen als base64). `index.html` is de app-pagina met de changelog ("What's new"), `support/index.html` de ondersteuning en `privacy/index.html` het privacybeleid van de app. De Nederlandse versies staan in `nl/`, `nl/support/` en `nl/privacy/`; bovenaan elke pagina wisselt een taalknop tussen de twee. Pas je de ene taal aan, doe dan de andere ook. Bij een nieuwe versie van Kaartjes komt er bovenaan de changelog een blok bij, in beide talen. Er staat bewust geen prijs op ("gratis"), want die ligt nog niet vast.
- Kaartjes is een soft launch: de pagina's staan live, maar de voorpagina linkt er (nog) niet naartoe. Zet die link er pas bij als daarom wordt gevraagd. De app is Nederlands en linkt het best naar `/kaartjes/nl/privacy/` en `/kaartjes/nl/support/`; de Engelse paden `/kaartjes/privacy/` en `/kaartjes/support/` blijven ook bestaan.
- `docs/CNAME` (agenticbeagle.com) en `docs/.nojekyll`: nodig voor GitHub Pages, niet weghalen.

## Regels

- De site is in het Engels. Alleen de Kaartjes-pagina's staan er ook in het Nederlands, omdat de app Nederlands is en alleen in Nederland te krijgen is.
- Niets van andere servers laden: geen Google Fonts, CDN, analytics, embeds of cookies. Nieuwe afbeeldingen en lettertypes gaan als base64 in de pagina. De privacypagina belooft dat; verandert er iets aan wat de site verwerkt, pas dan ook `docs/privacy.html` aan, met de datum bovenaan.
- Geen persoonlijke namen, privé-adressen of privé-e-mailadressen, op de site en in commits. Het contactadres is hello@agenticbeagle.com.
- AgenticBeagle is een B.V., ingeschreven bij de KvK onder nummer 42090573. De volledige naam "AgenticBeagle B.V." staat bij ENTITY op de voorpagina, in de footer (met het KvK-nummer, naast Privacy) en bij "Who we are" op de privacypagina; in lopende tekst blijft het gewoon AgenticBeagle.
- De merknaam staat in kleine letters met punt: `agenticbeagle.` (logo, voettekst, tabbladtitels, deeltitels). In lopende tekst en bij © blijft het `AgenticBeagle`. Verander dat niet zonder te vragen.
- Commits als `AgenticBeagle <hello@agenticbeagle.com>`, niet onder de eigen accountnaam. Dat staat per kopie in `.git/config`; controleer het met `git config user.name` voor de eerste commit, en zet het na een verse `git clone` eerst:
  ```bash
  git config user.name "AgenticBeagle"
  git config user.email "hello@agenticbeagle.com"
  ```
- Stijl: de kleuren en lettertypes staan als variabelen in `:root`. Bekijk elke wijziging ook op telefoonbreedte (375 px), zonder horizontaal scrollen.

## Werkwijze

`main` is live: wat in `main` komt, staat binnen een minuut op agenticbeagle.com. Daarom gaat elke wijziging zo:

1. **Begin:** `git switch main` en `git pull --ff-only`; de ander kan intussen iets live hebben gezet. Maak dan een branch met een korte Engelse naam in kebab-case, bijvoorbeeld `new-intro-text`. Eén wijziging per branch; nooit rechtstreeks op `main` werken.
2. **Maken en bekijken:** pas de bestanden in `docs/` aan en bekijk het resultaat met `python3 -m http.server 8000 --directory docs` (http://localhost:8000/): beide pagina's, op desktopbreedte en op 375 px. Vertel wat er veranderd is.
3. **"commit":** alleen op opdracht van wie aan het werk is, nooit uit eigen beweging. Commit op de branch, met een Nederlandse boodschap die zegt wat er veranderd is en waarom.
4. **"merge":** ook alleen op opdracht. Haal `main` op; is `main` intussen veranderd, rebase dan eerst de branch op `main`. Dan fast-forward naar `main`, `main` pushen en de branch weghalen. Branches blijven lokaal; alleen `main` gaat naar GitHub.
5. **Controleren:** kijk een paar minuten na de merge of https://agenticbeagle.com de nieuwe versie toont, en zeg in gewone taal wat er live staat. Stop daarna de lokale server.

**Eerst laten meekijken (optioneel):** vraagt degene die werkt daarom, push dan de branch en open een pull request met een korte uitleg in gewone taal (`gh pr create`). De ander bekijkt hem op GitHub en merget met "Rebase and merge"; GitHub haalt de branch daarna weg. Haal dan lokaal `main` op en verwijder de lokale branch.

**Terugdraaien:** gaat er na een merge iets mis, maak dan een nieuwe branch, draai de foute commit terug met `git revert` en merge die op dezelfde manier. Nooit force-pushen of de geschiedenis van `main` herschrijven; GitHub blokkeert dat ook.

Zonder server kan bekijken ook met `open docs/index.html`, maar dan werkt de link van de privacypagina terug naar de voorpagina niet.

## Afbeeldingen

Verklein een afbeelding eerst (bijvoorbeeld `sips --resampleWidth 840 foto.jpg`) en controleer dat er geen locatie, camera of naam in de metadata staat. Daarna als base64 in de pagina:

```bash
python3 -c "import base64,sys; print('data:image/jpeg;base64,' + base64.b64encode(open(sys.argv[1], 'rb').read()).decode())" foto.jpg
```

Houd elke pagina onder de 2 MB.
