# Responsive flexbox

Først, et lite tips til dere som bruker VS Code (og Mac): Bruk ⇧⌘V for å se preview av `.md`-filer

Poenget med denne oppgaven er å gjenskape det responsive flexbox-gridet på [strim.no](https://www.strim.no/play).

## Mobil

Vi starter med å gjøre de endringene som skal til for at siden ser bra ut på mobil. Åpne `index.html`-fila i en nettleser, og juster ned størrelsen på nettleservinduet til ca `550px`. Sammenlignet med bildet under er det et par ting som gjenstår: Vi vil ha litt avstand mellom hvert av "kortene",
og vi vil kutte ned teksten.

<img src="strim.png" width="400">

1. Sett avstanden på høyre og venstre side av kortene til `10px`, og avstanden mellom hvert kort til `1rem`.

For å kutte ned på teksten, så må vi trikse litt med `line-height` og `max-height`. `line-height` definerer hvor stor avstand det er mellom to linjer med tekst. Dersom den er satt til et dimensjonsløst tall (`x`), regnes den ut til å være `x*font-size`.

Default verdien til `line-height` er `normal`. Nøyaktig hvilken verdi dette tilsvarer avhenger av nettleser og font, men stort sett er det rundt 1,2 ganger fontstørrelsen.

2. Sett en eksplisitt `line-height` på elementene som inneholder beskrivelser.

### En liten digresjon om `px`, `em` og `rem`

- Størrelser definert i `px` er statiske
- Størrelser definert i `rem` forholder seg alltid til fontstørrelsen satt på html-elementet. For de fleste nettlesere er dette `16px` (med mindre man overskriver det).
- Størrelser definert i `em` forholder seg til fontstørrelsen på elementet. I utgangspunktet vil `1em` tilsvare `16px` for alle elementer, men dette gjelder bare enn så lenge vi ikke endrer en fontstørrelse noe sted, siden dette er en property som arves.

3. Sett `max-height` på det samme elementene inneholder beskrivelser. For å kutte teksten til akkurat tre linjer, må denne regnes ut på bakgrunn av `line-height`, og forholde seg til fontstørrelsen på en eller annen måte.
4. Bruk `overflow: hidden` for å skjule teksten som havner utenfor.
5. Hvis du vil teste hvordan man kanskje kan gjøre dette i "fremtiden", kan du forsøke å legge til følgende CSS nederst i selektoren:

```css
 {
  ... display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
}
```

Siden vi ønsker at kortene skal ligge ved siden av hverandre på større skjermer, trenger vi noe flexbox. La oss gjøre det med en gang.

6. Sett `display: flex` i `.container`-selektoren, og sett en property som gjør at vi unngår at alle kortene presses inn på én rad.

## Tablet

Begynn med å øke bredden på nettleseren til `575px` – her vil vi ha et breakpoint som gjør at vi får tre kort per rad.

1. Lag en `@media`-regel nederst i CSS-fila, som gjelder for nettleservinduer som er større enn `575px`.
2. Sett `flex-basis` til 33%, inne i `@media`-regelen. Avhengig av hvordan du har satt den horisontale avstanden mellom kortene på, så kan det hende at det nå bare er to elementer per rad.

Grunnen til at det er to elementer per rad, er fordi vi har sagt at bredden (`flex-basis`) til hvert element, skal være 33%. Totalt vil da tre elementer utgjøre 99% av bredden, men vi har jo lagt til litt spacing også, og dette gjør at det tredje elementet dyttes ned på siden. Det er to måter å løse dette problemet på:

3. - Bruk `padding` for å sette spacing mellom elementene
   - Bruk [`calc()`](https://developer.mozilla.org/en-US/docs/Web/CSS/calc) til å regne ut `flex-basis`. Hvert element skal tilsvare `100% / 3`, minus så mye margin som "tilhører" hvert kort.

Det kan være greit å notere seg at å sette `padding` ikke nødvendigvis funker i alle tilfeller. Gitt at kortet skal ha en ramme f.eks. Da må man fort lage en wrapper rundt kortet igjen, som kun bestemmer `padding` og `flex-basis`.

Et annet "problem" er at en side gjerne har en uniform padding langs kanten. Hvis vi setter `margin` eller `padding` på hvert element, vil de elementene lengst til høyre og venstre gjøre at hele containeren får litt ekstra avstand til kanten. For å fikse dette er det et kjent triks å bruke negativ margin på containeren, som tilsvarer den spacingen som er satt på på hvert kort. Dette ble kanskje litt knotete forklart, så spør gjerne hvis du vil ha en demo.

## Desktop

For desktop ønsker vi at det skal være fire kort i bredden, og vi ønsker at dette skal skje når skjermen blir større enn 768px.

1. Lag en `@media`-regel til, og legg inn den CSSen du trenger for at det skal bli fire kort i bredden.
2. Sett en regel som gjør at containeren aldri blir mer enn `1070px` bred. Pass også på at den sentreres dersom siden blir bredere enn dette.
