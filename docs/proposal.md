# Predlog projekta — Shelfie

**Shelfie** je informacijski sistem za upravljanje manjših, neformalnih knjižnic v študentskih domovih, društvih in klubih. Člani prek spleta ali telefona iščejo po katalogu ter si izposojajo, rezervirajo in podaljšujejo knjige. Skrbniki (običajno prostovoljci) v njem vodijo zbirko in izposojo.

## Problem
Študentski domovi, društva in klubi imajo pogosto svoje manjše knjižnice, ki jih vodijo prostovoljci v preglednicah ali pa sploh ne, zato knjige izginjajo in nihče ne ve, kdo ima kaj. Sistemi, kot je COBISS, so namenjeni pravim knjižnicam in so za takšne zbirke predragi in prezapleteni. Shelfie jim omogoča enostavno vodenje izposoje, rezervacij in rokov vračila, člani pa do knjig dostopajo tudi s telefonom.

## Uporabniki in vloge
| Vloga | Opis | Kaj lahko počne |
|-------|------|-----------------|
| Član | Stanovalec doma ali član društva/kluba | Išče po katalogu, vidi razpoložljivost, rezervira knjige, pregleduje in podaljšuje svoje izposoje, uporablja mobilno aplikacijo in AI asistenta |
| Skrbnik | Prostovoljec, ki skrbi za knjižnico | Upravlja knjige in fizične izvode, izposoja in sprejema vrnjene izvode, upravlja rezervacije, pregleduje izposoje z zamudo, začasno blokira ali odblokira člane |
| Administrator | Upravitelj knjižnice v sistemu (npr. predsednik društva) | Upravlja uporabniške račune in vloge, nastavlja pravila izposoje (trajanje izposoje, največje število izposoj, največje število podaljšanj) |

Avtorizacija se izvaja na strežniku glede na vlogo **in** lastništvo: član lahko vidi in spreminja samo svoje izposoje in rezervacije.

## User stories

Format: *As a <role>, I want <goal>, so that <benefit>.* Priority: **M**ust / **S**hould / **C**ould / **W**on't.
Create a GitHub issue for each story (use the "User story" issue template) and link it here.

| # | Story | Priority | Milestone | Issue |
|---|-------|----------|-----------|-------|
| 1 | As a …, I want …, so that … | M | midterm | #… |
| 2 | | | | |

### Acceptance criteria (example for story 1)
- **Given** …
- **When** …
- **Then** …

## Uporaba mobilne aplikacije
Mobilni odjemalec bo **progresivna spletna aplikacija (PWA)**, ki si jo uporabnik namesti na začetni zaslon telefona in uporablja isti API kot spletna aplikacija. Telefon se uporablja **pred knjižno polico in na poti**. To je še posebej pomembno, ker v neformalnih knjižnicah skrbnik pogosto ni prisoten:

- **Skeniranje črtne kode ISBN** s kamero telefona: takoj prikaže razpoložljivost in čakalno vrsto, knjigo pa je mogoče rezervirati ali izposoditi z enim dotikom.
- **»Moje izposoje«** na poti: roki vračila, podaljšanje z enim dotikom in stanje rezervacij, ki so na voljo tudi brez povezave (zadnje shranjeno stanje).
- **Potisna obvestila** (zaželeno): ko je rezervirana knjiga pripravljena za prevzem ali ko se izteka rok izposoje.

## AI funkcionalnosti
- **Funkcionalnost z LLM: iskanje po katalogu v naravnem jeziku.** Član vpiše prosto besedilo (»lahek uvod v strojno učenje v slovenščini«). LLM ga pretvori v strukturiran filter (tema/žanr, ključne besede, jezik, zahtevnost), ki se izvede na našem iskalnem endpointu. Rezultati so razvrščeni, vsak pa ima enovrstično razlago, zakaj ustreza iskanju.
- **Naloga agenta:** *»Poišči razpoložljivo knjigo na temo, ki jo opišem, jo rezerviraj in podaljšaj moje izposoje, ki se jim izteka rok — po moji potrditvi.«* Agent uporablja naš API (iskanje po katalogu → preverjanje razpoložljivosti → seznam mojih izposoj → rezervacija → podaljšanje). Dejanja, ki spreminjajo podatke, zahtevajo izrecno potrditev uporabnika, kar je uveljavljeno v kodi, vsak klic pa se izvede z žetonom prijavljenega člana.
