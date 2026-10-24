# agenticbeagle.com

De website van AgenticBeagle. Hij staat op GitHub Pages: de branch `main`, map `/`, met het eigen domein agenticbeagle.com (`CNAME`). Er zijn twee accounts die eraan werken: DevBeagle (lokaal, op de Mac) en HelloBeagle (Claude Code op het web).

## Bestanden

- `index.html`: de voorpagina. Alles zit in dit ene bestand: de CSS, de afbeeldingen en de drie lettertypes (Newsreader, Inter en JetBrains Mono; SIL Open Font License) staan er als base64 in. Er is geen JavaScript.
- `privacy.html`: de privacypagina, in dezelfde stijl en met een eigen kopie van de lettertypes. Onderaan de voorpagina staat de link "Privacy".
- `CNAME` (agenticbeagle.com) en `.nojekyll`: nodig voor GitHub Pages, niet weghalen.

## Regels

- De site is alleen in het Engels.
- Niets van andere servers laden: geen Google Fonts, CDN, analytics, embeds of cookies. Nieuwe afbeeldingen en lettertypes gaan als base64 in de pagina. De privacypagina belooft dat; verandert er iets aan wat de site verwerkt, pas dan ook `privacy.html` aan, met de datum bovenaan.
- Geen persoonlijke namen, privé-adressen of privé-e-mailadressen, op de site en in commits. Het contactadres is hello@agenticbeagle.com.
- AgenticBeagle is geen B.V.; schrijf dat nergens.
- Commits als `AgenticBeagle <hello@agenticbeagle.com>`. Lokaal staat dat al in `.git/config`. In een nieuwe omgeving, zoals Claude Code op het web, eerst:
  ```bash
  git config user.name "AgenticBeagle"
  git config user.email "hello@agenticbeagle.com"
  ```
- Stijl: de kleuren en lettertypes staan als variabelen in `:root`. Bekijk elke wijziging ook op telefoonbreedte (375 px), zonder horizontaal scrollen.

## Werkwijze

- `main` is live: wat in `main` komt, staat binnen een minuut op agenticbeagle.com.
- Elke wijziging op een eigen branch, met een korte Engelse naam in kebab-case. Nooit rechtstreeks op `main` werken en nooit force-pushen.
- Lokaal (DevBeagle): eerst `git pull --ff-only` op `main`, want er wordt ook vanuit de cloud gewerkt. Committen en mergen alleen op uitdrukkelijke opdracht ("commit", "merge"). Merge betekent: fast-forward naar `main`, `main` pushen en de branch weghalen. Alleen `main` gaat naar GitHub.
- In de cloud (HelloBeagle): de branch pushen en een pull request openen met een korte uitleg in gewone taal. Zij bekijkt het en merget met "Rebase and merge"; daarna verdwijnt de branch vanzelf.
- Bekijken: `python3 -m http.server 8000` in deze map, dan http://localhost:8000/. Met `open index.html` gaat het ook, maar dan werkt de link van de privacypagina terug naar de voorpagina niet.

## Afbeeldingen

Verklein een afbeelding eerst (bijvoorbeeld `sips --resampleWidth 840 foto.jpg`) en controleer dat er geen locatie, camera of naam in de metadata staat. Daarna als base64 in de pagina:

```bash
python3 -c "import base64,sys; print('data:image/jpeg;base64,' + base64.b64encode(open(sys.argv[1], 'rb').read()).decode())" foto.jpg
```

Houd elke pagina onder de 2 MB.
