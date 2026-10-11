# CREEPYSTORIES

Adam Jelža

5ZYI36

## Stručný opis projektu

Creepystories je komunitná webová aplikácia určená predovšetkým slovenským, prípadne aj českým čitateľom a autorom hororových príbehov, známych ako *creepypasty*. Cieľom je vytvoriť jedno prehľadné miesto, kde si návštevníci môžu príbehy vyhľadávať a čítať, zatiaľ čo registrovaní používatelia môžu zverejňovať vlastnú tvorbu, komentovať príbehy a hodnotiť ich. Aplikácia bude navrhnutá s dôrazom na čitateľnosť, jednoduchú orientáciu a použiteľnosť aj na mobilných zariadeniach.

## Role v projekte

- **Návštevník:** Môže prehliadať zverejnené príbehy, čítať ich, vyhľadávať podľa názvu či autora, zobrazovať profily autorov.
- **Registrovaný používateľ:** Má možnosti návštevníka. Navyše môže vytvárať a spravovať vlastné príbehy, písať a upravovať svoje komentáre.
- **Moderátor:** Kontroluje príbehy čakajúce na schválenie a rozhoduje o ich prijatí alebo zamietnutí.
- **Administrátor:** Spravuje používateľské účty a ich roly a obsah aplikácie. Má tiež oprávnenia potrebné na administráciu a moderovanie.

## Prípady použitia podľa rolí

**Návštevník**
- Registrácia nového účtu
- Vyhľadá príbeh podľa názvu alebo autora.
- Otvorí detail zverejneného príbehu, prečíta si ho a zobrazí komentáre či hodnotenie.
- Zobrazí profil autora a jeho publikované príbehy.

**Registrovaný používateľ**
- Prihlási sa a upraví údaje svojho profilu.
- Vytvorí príbeh, uloží rozpracovaný príbeh, upraví alebo odstráni ho a odošle ho na schválenie.
- Pridá, upraví alebo odstráni vlastný komentár.
- Ohodnotí publikovaný príbeh známkou od 1 do 10 a môže svoje hodnotenie zmeniť.

**Moderátor**
- Zobrazí zoznam príbehov čakajúcich na schválenie.
- Prečíta si odoslaný príbeh a schváli ho alebo zamietne.

**Administrátor**
- Zobrazí a spravuje používateľov a ich oprávnenia.
- Spravuje obsah a vykonáva moderátorské úkony.

## Plánované entity

- **`users` – používatelia:** Prihlasovacie údaje a rola používateľa. Hlavné atribúty: `id`, `username`, `email`, `password`, `role`, `created_at`.
- **`stories` – príbehy:** Texty vytvorené používateľmi a ich stav schvaľovania. Hlavné atribúty: `id`, `user_id`, `title`, `content`, `status` (`draft`, `pending`, `published`, `rejected`), `created_at`, `published_at`, `updated_at`.
- **`comments` – komentáre:** Komentáre používateľov k jednotlivým príbehom. Atribúty: `id`, `story_id`, `user_id`, `content`, `created_at`, `updated_at`.
- **`ratings` – hodnotenia príbehov:** Číselné hodnotenie príbehu od 1 do 10. Atribúty: `id`, `story_id`, `user_id`, `value`, `created_at`. Jeden používateľ môže mať pri konkrétnom príbehu najviac jedno aktuálne hodnotenie.

Samostatné tabuľky `stories`, `comments` a `ratings` sú zmysluplné entity aplikácie. 

## Vzťahy medzi entitami

- **1:N – `users` → `stories`:** Jeden používateľ môže napísať viacero príbehov, každý príbeh má jedného autora.
- **1:N – `users` → `comments`:** Používateľ môže napísať viacero komentárov.
- **1:N – `stories` → `comments`:** Jeden príbeh môže obsahovať viacero komentárov.
- **1:N – `stories` → `ratings` a `users` → `ratings`:** Príbeh môže dostať viacero hodnotení od rôznych používateľov. Kombinácia `user_id` a `story_id` bude jedinečná.

## Hlavné stránky aplikácie

1. **Domovská stránka:** Úvod aplikácie s výberom najnovších a najlepšie hodnotených publikovaných príbehov.
2. **Prehľad príbehov:** Dynamický zoznam publikovaných príbehov s vyhľadávaním.
3. **Detail príbehu:** Text príbehu, informácie o autorovi, hodnotenia, komentáre.
4. **Editor príbehov:** Formulár na vytvorenie a úpravu príbehu, uloženie alebo odoslanie na schválenie.
5. **Moje príbehy:** Prehľad vlastných príbehov rozdelených podľa stavu (rozpracované, čakajúce na schválenie, zverejnené, zamietnuté).
6. **Profil používateľa:** Informácie o používateľovi a zoznam jeho zverejnených príbehov; vlastník môže upraviť svoj profil.
7. **Moderácia:** Zoznam čakajúcich príbehov s detailom a možnosťou schválenia alebo zamietnutia.
8. **Administrácia:** Správa používateľov a rolí.

Aplikácia bude obsahovať aj registračnú a prihlasovaciu stránku. Prístup k editoru, vlastným príbehom, moderácii a administrácii bude obmedzený podľa prihlásenia a oprávnení.

## Rozdelenie funkcionality

**Základné funkcie (plánované na dokončenie semestrálnej práce):**

- Serverová časť v **PHP s frameworkom VAJKO** a oddelenie aplikačnej logiky od zobrazovania.
- Registrácia, prihlásenie, odhlásenie a autorizácia podľa používateľskej roly.
- CRUD operácie minimálne nad 2 tabuľkami (`stories`, `comments`).
- Dve zmysluplné asynchrónne operácie so serverom (**AJAX**): vyhľadávanie príbehov a odosielanie alebo zmena hodnotenia bez obnovenia stránky.
- Overovanie vstupov na strane klienta aj servera, ochrana pred SQL injection a XSS, bezpečné uloženie hesiel a kontrola oprávnení.
- Responzívne rozhranie s vlastným CSS a vlastným JavaScriptom v rozsahu vyžadovanom zadaním.
- Spustenie aplikácie pomocou **Dockeru** vrátane automatického vytvorenia alebo naplnenia databázy ukážkovými údajmi pri prvom spustení.
- Schvaľovanie alebo zamietanie príbehov moderátorom pred verejným zobrazením.
- Hodnotenie príbehov od 1 do 10.
- Ukladanie rozpracovaných príbehov.

**Rozširujúce funkcie (podľa časových možností):**

- Pokročilé triedenie a filtrovanie príbehov podľa hodnotenia, dĺžky a popularity.
- Štatistiky autora, napríklad počet publikovaných príbehov a ich priemerné hodnotenie.
- Odhadovaný čas čítania príbehu.
- Prepínanie medzi tmavým a svetlým režimom.
- Ukladanie rozčítanej pozície v prehliadači pomocou Web Storage API.
- **Práca so súbormi:** nahrávanie, zobrazenie a odstránenie titulného obrázka príbehu s kontrolou typu a veľkosti súboru.
- Markdown editor so živým náhľadom formátovania textu.
- Pridávanie tagov k príbehom a filtrovanie vyhľadávania pomocou nich.
- Reakcie like/dislike na komentáre.
