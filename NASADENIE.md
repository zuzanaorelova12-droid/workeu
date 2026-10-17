# Nasadenie WorkEU na Render (krok za krokom)

## 1. GitHub
1. Vytvorte si účet na github.com a nový **súkromný** repozitár (napr. `workeu`).
2. Cez „Add file → Upload files“ nahrajte **obsah** priečinka `workeu-proxy`
   (`server.js`, `package.json`, `render.yaml`, `test.js`, `README.md`, priečinok `public`).
3. **Nenahrávajte** súbor `.env` s kľúčmi. Používame ho len na vlastnom počítači.

## 2. Render
1. Zaregistrujte sa na render.com a prepojte účet s GitHubom.
2. **New → Blueprint** → vyberte repozitár `workeu`. Render si načíta `render.yaml`
   (región Frankfurt, spustí `npm test` a `npm start`).
3. Pri premenných prostredia vyplňte:
   - `ARBEITNOW_ENABLED` = `true` (už predvyplnené)
   - Bundesagentur je vypnutá (`BA_API_KEY` nenastavujte), ponuky z nej sa nezobrazujú
   - `ADZUNA_APP_ID` a `ADZUNA_APP_KEY` = prázdne (pridáte neskôr)
4. Kliknite **Apply / Deploy**. Prvé nasadenie trvá pár minút.

## 3. Kontrola
Otvorte adresu, ktorú Render zobrazí (napr. `https://workeu.onrender.com`):
- `/api/health` má ukázať, ktoré zdroje sú zapnuté,
- `/api/jobs?q=Lager` má vrátiť ponuky,
- hlavná stránka má po načítaní zmiznúť upozornenie „Zobrazujú sa ukážkové ponuky“.

Ak sa niečo nezobrazuje správne, pošlite mi odpoveď z `/api/jobs?q=Lager`
(sekcia `sources` ukáže, ktorý zdroj zlyhal) a upravím mapovanie.

## 4. Neskôr
- Plán `starter` beží stále. Bezplatný plán po čase nečinnosti „zaspí“.
- Vlastná doména: v Renderi Settings → Custom Domains.
- Pridanie kľúčov: Render → Environment, potom Manual Deploy.
- Pred verejným spustením: preverte podmienky použitých zdrojov (Arbeitnow, Adzuna)
  a doplňte ich do Zásad ochrany súkromia.

## 5. Preklad v „Nerozumiem“ (voliteľné)
Bez kľúča AI funguje len ukážka (pár fráz a slovíčka z lekcií). Aby aplikácia preložila a vysvetlila
hociktoré slovo či vetu do zvoleného jazyka, na Renderi doplňte premennú `ANTHROPIC_API_KEY`
(kľúč si vytvoríte v konzole Anthropic). Kľúč je platený podľa použitia, preto je v serveri
limit požiadaviek. Text, ktorý kandidát napíše, sa odošle poskytovateľovi AI. Uveďte to v Zásadách
ochrany súkromia. Kľúč nikomu neposielajte, ani mne.
