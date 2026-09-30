# AGENTS.md – Pravidla a zásady pro vývojového agenta

Tento dokument definuje závazná pravidla chování, pracovní postupy a omezení pro AI vývojového agenta v repozitáři **`ppro2026`** (Předmět: *Pokročilé programování*, FIM UHK, ZS 2026/2027).

---

## 1. Role a kontext projektu

- **Projekt**: Informační a rezervační systém pro ordinaci – **Zadání A: Rezervace v ordinaci**.
- **Klient**: MUDr. Jana Hrubá, Ordinace Na Vyhlídce.
- **Uživatelé**: Recepční paní Věra, lékaři na částečný úvazek, pacienti.
- **Cíl**: Návrh a implementace robustní třívrstvé aplikace splňující veškeré funkční požadavky, obchodní pravidla a povinné minimum předmětu PPRO.

---

## 2. Závazné zásady a operační pravidla

### A. Jediný zdroj pravdy (Single Source of Truth)
- Soubor [`README.md`](file:///u:/ppro2026/README.md) je **výhradním a autoritativním zdrojem pravdy** celého projektu.
- Slouží současně jako:
  1. Kompletní specifikace zadání klienta (požadavky, obchodní pravidla, otevřené body).
  2. Technická a architektonická dokumentace (vrstvy, datový model, rozhraní, Docker, konfigurace).
  3. **Evidence rozhodnutí (Decisions Log)** ke všem otevřeným bodům i budoucím změnám zadání.
  4. **Záznam o postupu prací (Changelog / Progress Log)**.
- **Pravidlo**: Žádná architektonická, doménová nebo funkční změna nesmí vzniknout v kódu, aniž by byla nejprve nebo současně precizně zanesena a vysvětlena v [`README.md`](file:///u:/ppro2026/README.md).

### B. Pravidla pro práci s Git
1. **Zákaz commitu bez zprávy**:
   - Každý commit MUSÍ být proveden s přepínačem `-m` a jasnou, věcnou a strukturovanou zprávou popisující podstatu a účel změny (např. ve formátu Conventional Commits: `feat: ...`, `fix: ...`, `docs: ...`, `refactor: ...`, `test: ...`).
   - Zakázány jsou obecné nebo vágní zprávy typu "update", "wip", "changes".
2. **Aktualizace dokumentace před každým commitem**:
   - Před spuštěním `git commit` je agent povinen se ujistit, že technická dokumentace a záznam o postupu v [`README.md`](file:///u:/ppro2026/README.md) jsou plně aktuální a zahrnuté do commitu (stageovány).
   - K dodržení tohoto pravidla slouží také lokální Git hook (`pre-commit`).
3. **Striktní zákaz automatického pushování**:
   - Agent **NIKDY nesmí** provést `git push` automaticky, iniciativně ani na základě vlastního uvážení.
   - Provedení `git push` je povoleno **VÝHRADNĚ na základě explicitního, přímého příkazu uživatele** v konverzaci (např. „Proveď push“, „Pushni změny na GitHub“).

---

## 3. Technické a architektonické standardy (PPRO Minimum)

Agent je povinen garantovat splnění povinného minima semestrálního projektu:
1. **Třívrstvá architektura**:
   - Striktní závislosti jedním směrem: `Prezentační vrstva (Web UI / API)` $\to$ `Aplikační / Byznys vrstva (Services, doménová logika)` $\to$ `Datová / Perzistentní vrstva (Entity, Repositories)`.
2. **Relační databáze v Dockeru**:
   - Databáze běží v kontejneru definovaném v `docker-compose.yml`.
   - Schéma je spravováno výhradně přes databázové migrace (žádné manuální zásahy do DB).
   - Spuštění celého prostředí musí být realizovatelné jediným příkazem: `docker compose up`.
3. **Doménový model**:
   - Nejméně 5 doménových entit.
   - Nejméně jedna vazba M:N (v projektu: vazba mezi `Návštěvou` a `Výkonem` nesoucí atribut počtu provedení).
4. **Automatizované testy**:
   - Důraz na unit a integrační testy pro kritická obchodní pravidla (překryvy termínů, ordinační hodiny, nemožnost zapsat výkon k budoucí návštěvě, integrita historie).
5. **Čistota a bezpečnost dat**:
   - Striktně syntetická testovací data (žádná reálná rodná čísla, jména ani lékařská data).
   - Žádná hesla, privátní klíče ani citlivé konfigurační údaje v repozitáři.

---

## 4. Reakce na změny zadání od klienta

- V průběhu semestru bude klient (vyučující na cvičení) přicházet s upřesněními či změnami požadavků.
- **Postup agenta při změně**:
  1. Analyzovat dopad požadavku na datový model a obchodní pravidla.
  2. Zapsat změnu a její zdůvodnění do sekce *Rozhodnutí* v [`README.md`](file:///u:/ppro2026/README.md).
  3. Teprve po aktualizaci specifikace upravit migrace, kód a testy.
