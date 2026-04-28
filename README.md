# Slevomat Design Check

> AI skill, který posuzuje featuru, obrazovku nebo screenshot mockupu proti **7 designovým principům Slevomatu**.
> Pure text/image-in → strukturovaný feedback-out. Žádné MCP, žádné generování souborů.

**Status:** v0.1.0 (2026-04-28)
**Repo visibility:** Public — sdílíme open jako příspěvek do design komunity.
**Owner:** Michal Strnadel (Design Lead, Slevomat) — michal.strnadel@slevomat.cz

---

## Proč to existuje

Designových rozhodnutí dělá ve Slevomatu denně spousta lidí — PMs, designéři, devs, stakeholdeři. Aby produkty působily konzistentně, musíme všichni vycházet ze stejné definice toho, co je „kvalitní design".

Tým designu má v hlavě 7 principů z interního workshopu. Tento skill je **škáluje na celou firmu**: každý PM si může nechat svůj nápad nebo screen projet stejnou optikou, kterou dnes drží jen 3 designéři. To znamená:

- méně review schůzek pro design tým,
- konzistentnější rozhodování napříč produktem,
- rychlejší filtraci slabých nápadů (než spotřebují design hodiny).

## Co je uvnitř

Tento repo obsahuje **2 soubory**, které jsou taženy ze [slevomat-ai-hub](https://github.com/slevomat/design-slevomat-context) (interní hub všech AI skillů). Tady jsou samostatně, aby se daly snadno sdílet, citovat nebo nasadit do izolované Claude Code session.

| Soubor | Co obsahuje |
|---|---|
| [`design-principles.md`](./design-principles.md) | Kanonická verze 7 designových principů Slevomatu (Co znamená / Proč / Jak se pozná v praxi). Source of truth pro skill. |
| [`SKILL.md`](./SKILL.md) | AI skill: instrukce, role, examples, anti-patterns. Frontmatter kompatibilní s Claude.ai uploadem (jen `name` + `description`). |
| [`PROMPT.md`](./PROMPT.md) | **Single-file portable prompt** — skill + principy zlepené do jednoho ready-to-paste promptu pro Claude.ai / Gemini / ChatGPT. Použij když nejde stáhnout repo přímo. |
| [`slevomat-design-check.zip`](./slevomat-design-check.zip) | **Předbalený ZIP pro Claude.ai upload** (Skills → + → Upload a skill). Obsahuje SKILL.md + design-principles.md ve správné struktuře. |

---

## 7 designových principů — přehled

1. **Použitelnost a spolehlivost jako základní zážitek** — design intuitivní, rychlý, předvídatelný; přístupnost (WCAG AA) je default
2. **Vizuální kultivovanost** — i bohatá nabídka může vypadat přehledně; nepůsobíme jako blikající leták
3. **Zřetelně výhodně** — výhodnost má místo, ale **nemusí křičet**; férovost, ne triky
4. **Zážitek bez přikrášlení** — autentický design, kterému se dá věřit; ne marketingová bublina
5. **Designujeme cestu uživatele, ne jen obrazovky** — celý journey, ne izolované UI; UX research je základ
6. **Ukazujeme směr a necháváme objevovat** — vedeme, ale netlačíme; balancujeme directive s volností
7. **Design, který překvapí** — promyšlené momenty, které dávají zážitku hloubku; emoce, ne efekty

Plný popis každého principu (Co znamená / Proč / Jak se pozná v praxi) je v [`design-principles.md`](./design-principles.md).

---

## Jak skill používat

### Předpoklady

- **Claude Code** ([instalace](https://docs.claude.com/en/docs/claude-code/quickstart))
- Repo naklonovaný lokálně, nebo soubory umístěné kam ti to v repu vyhovuje

### Registrace skillu

Skill je SKILL.md ve formátu Claude Code. Pokud používáš **slevomat-ai-hub**, skill je automaticky aktivovatelný. V samostatném použití (tento repo):

1. Naklonuj repo: `git clone <repo-url> && cd slevomat-design-check`
2. Spusť Claude Code: `claude` ve složce repa
3. V chatu napiš: `design-check` + popis nebo přilož screenshot

### Použití mimo Claude Code

#### Claude.ai (s nahráním do Skills)

1. Stáhni [`slevomat-design-check.zip`](./slevomat-design-check.zip) (klik na soubor → tlačítko **Download** vpravo nahoře)
2. V Claude.ai → Skills panel → **+** → **Create skill** → **Upload a skill**
3. Vyber stažený ZIP
4. Skill se objeví v Personal skills, aktivuje se napsáním `design-check` v konverzaci

#### Claude.ai / Gemini / ChatGPT (bez nahrání — paste prompt)

Pokud nechceš nahrávat skill, máš **2 možnosti**:

**A) Nech model stáhnout repo:**
> "Načti SKILL.md a design-principles.md z https://github.com/michalstrnadel/slevomat-design-check a aplikuj skill na tenhle screenshot."

Tohle občas selže kvůli **GitHub anonymous rate limitu** (zvlášť pod zátěží).

**B) Single-file paste (spolehlivější):**
1. Otevři [`PROMPT.md`](./PROMPT.md) v repu
2. Zkopíruj **celý obsah** (Raw view → Cmd+A, Cmd+C)
3. Paste do první zprávy v chatu
4. Druhá zpráva: screenshot + popis

`PROMPT.md` má skill + principy v jednom souboru, takže model nepotřebuje nic stahovat.

### Spouštěcí fráze (triggery)

- `design-check`
- `projeď přes designové principy`
- `design review`
- `ověř proti principům`
- `slevomat design principy`
- `principy check`

### Vstupy, které skill přijme

- **Text** — popis featury, user story, funkční zadání
- **Screenshot / obrázek** — mockup, hotová obrazovka, Figma export
- **Hybridní** — text + obrázek

### Co dostaneš zpátky

Kompaktní strukturovaný report (~250 slov detailu):

1. **Hodnocení tabulka** — všech 7 principů s emoji verdikty
2. **Detail** — pro každý dotčený princip (✅ Hit / ⚠️ Concern / ❌ Violation):
   - Krátká citace z principu, který byl porušen / naplněn
   - Co konkrétně ve vstupu k tomuhle vede (1–2 věty)
   - Konkrétní doporučení (1 věta)
3. **Skóre** — kolik Hit / Concern / Violation / Cannot tell
4. **Top 3 akce** — seřazené podle dopadu, ne podle čísla principu
5. **Verdikt** — `GO` / `SHARPEN` / `HOLD` / `NEED MORE INFO`

Principy s 🤷 Cannot tell se v detailu nezobrazují — zůstávají jen v sumární tabulce, ať je report kompaktní.

---

## Klíčové behaviorální pravidlo

> **Cannot tell ≠ Hit.**
> Skill nesmí zakrývat nedostatek důkazů hodnocením `Hit`. Pokud ze vstupu nejde posoudit, je férovější vrátit 🤷 Cannot tell s vysvětlením, co chybí.

---

## Kdo skill používá

- **PMs** — před tím, než dají nápad do PRD: „stojí za to to vůbec rozpracovat?"
- **Designéři (Marián, Jirka)** — self-review před sdílením nebo handoff
- **Exec / managers** — gate-check na schůzce
- **Design Lead (Michal)** — triage co tým bude designovat příště

---

## Pending / chybí v podkladech

Tento skill je v0.1. Aby byl ostřejší, design tým by měl doplnit:

- **Konkrétní personas** (first-time travel buyer, loyal heavy user, gift shopper) pro inference kontextu
- **Customer journey mapy** pro princip #5 (existuje jen prázdný stub)
- **Design system / komponentová knihovna** — aby skill věděl baseline („tohle není Slevomat tlačítko")
- **Brand tokeny** (real barvy a fonty z brand booku) — momentálně placeholder hodnoty
- **WCAG checklist** — dnes skill používá generický WCAG AA standard
- **Copywriter pass přes principy** — pročistit opakované fráze (per workshop feedback)
- **Redukce cross-principle opakování** — některé body v praxi-bulletech říkají totéž jiným způsobem napříč principy (per workshop feedback)

---

## Aplikovaný feedback z workshopu

Při ingestování principů do `design-principles.md` byl zpracovaný **červený feedback z workshopových lístečků**:

- **#1** — doplněna sekce o **přístupnosti** (WCAG AA, kontrast, klávesnice, screen readery). Původně chybělo.
- **#2** — opraveno z „texty jsou krátké, srozumitelné..." na „texty jsou srozumitelné a..." (Admin Slevomat: text nemusí být krátký, aby byl pochopitelný).
- **#3** — odstraněno slovo „**červeně**" ze „nemusí křičet červeně" → „nemusí křičet" (vymezovalo se to vůči konkrétní existující verzi, což do principů nepatří).
- **#7** — pročištěné opakované „blikající efekty" (z 2 výskytů na 1).

Zelené komentáře (Barbora Kejvalová: „Srozumitelně a jasně popsaný :-)") nevyžadovaly změnu.

---

## Vztah k hlavnímu repu

Tento standalone repo je **export ze [slevomat-ai-hub](https://github.com/slevomat/design-slevomat-context)** — interního hubu pro všechny AI skilly Slevomatu. Pokud máš přístup do hubu, používej ho odtamtud (skill je integrovaný s ostatními a sdílí `base/` reference). Tento standalone repo slouží pro:

- Jednoduché sdílení skillu (např. s externím konzultantem nebo na konferenci)
- Lehké nasazení do izolované Claude Code session
- Demo / prezentaci principů a skillu mimo interní kontext

**Source of truth zůstává hub** — pokud se principy aktualizují, dělá se to v hubu. Tento repo je periodicky resyncován (zatím manuálně).

---

## Bezpečnost a osobní údaje

- Repo je **public** — design principy sdílíme otevřeně. Konkrétní firemní data (zákaznické insights, byznys metriky, customer journey detaily) v tomto repu nejsou.
- Skill je instruovaný **nezveřejňovat osobní údaje** (skutečná jména, emaily, telefony, čísla objednávek), které by mohly být na screenshotech. V reportu označí jen jednou větou, že byly viděny.
- Při sdílení reportu (zvlášť veřejně) vždy zkontroluj, že neobsahuje citlivá data ze screenshotu.

---

## Licence

© Slevomat Group, s.r.o. Sdíleno jako open reference pro design komunitu — můžeš číst, citovat, inspirovat se. Pokud principy přebíráš pro vlastní projekt, prosím odkazuj zpět na tento repo a uveď zdroj. Pro komerční přepoužití nebo uvedení v publikaci kontaktuj michal.strnadel@slevomat.cz.

---

*Sestaveno společně s Claude Code (Opus 4.7), 2026-04-28.*
