# FRAM Avgangstavle — Buss i Ålesund

En moderne webapplikasjon (Vue 3 / Vite) som viser sanntidsinformasjon for buss-stopp i Ålesund ved bruk av Entur's JourneyPlanner v3 GraphQL API.

## Stoppesteder som vises
- **Øfstisvingen**
- **Nørvegata vest**
- **Blixvalen**

## Hvordan kjøre lokalt
1. Installer avhengigheter:
   ```bash
   npm install
   ```
2. Start utviklingsserver:
   ```bash
   npm run dev
   ```

## Oppsett på Raspberry Pi
Appen er testet og fungerer utmerket som en informasjonstavle på Raspberry Pi.

1. **Installer Node.js & npm**:
   ```bash
   sudo apt update
   sudo apt install nodejs npm
   ```
2. **Kjør appen**:
   Vi anbefaler å bruke en produksjons-build for best ytelse på en Pi:
   ```bash
   npm run build
   npx serve -s dist
   ```
3. **Kiosk Mode**:
   For å vise appen i fullskjerm uten menyer, bruk Chromium i kiosk-modus:
   ```bash
   chromium-browser --kiosk http://localhost:5173
   ```

## Teknisk info
- **Framework**: Vue 3 (Composition API)
- **Byggeverktøy**: Vite
- **Språk**: TypeScript
- **Styling**: Vanilla CSS (Inspirert av FRAM-farger)
- **API**: Entur JourneyPlanner v3 (GraphQL)
