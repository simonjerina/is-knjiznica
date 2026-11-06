# 0001. Tehnološki sklad: Nuxt + SQLite + Drizzle

- **Status:** accepted
- **Date:** 2026-11-06
- **Deciders:** Simon Jerina, Matej Suhadolc

## Context
Gradimo spletno aplikacijo, REST API, mobilnega odjemalca, LLM funkcionalnost in agenta, v 9 tednih in v dvoje. Pomembno nam je:
- čim manj ločenih projektov in jezikov (ekipa 2 osebi),
- isti API za splet, mobilno aplikacijo in agenta,
- enostavna lokalna postavitev in poceni gostovanje,
- relacijska baza z migracijami (zahteva predmeta).

## Options considered
1. **Nuxt (Vue + Nitro) full-stack, SQLite, Drizzle** — en jezik (TypeScript) in en projekt za frontend in API; PWA z dodatkom; SQLite ne potrebuje strežnika baze; Drizzle je tipsko varen, blizu SQL, z migracijami. *Slabosti:* SQLite ima enega zapisovalca in potrebuje trajni disk v oblaku; manj ločenosti med frontendom in API-jem.
2. **ASP.NET Core Web API + EF Core + PostgreSQL, ločen React/Vue frontend** — zrel ekosistem, močna ločitev slojev. *Slabosti:* dva jezika in dva projekta, več dela s postavitvijo.
3. **Express/NestJS + Prisma + PostgreSQL, ločen frontend** — en jezik. *Slabosti:* še vedno dva projekta in upravljana PostgreSQL baza (strošek, dodatna storitev).

Za ORM smo primerjali še **Prisma** (zrela, a težji runtime in lasten jezik sheme) in **Drizzle** (shema v TypeScriptu, lahek, dobra podpora za SQLite in libSQL).

## Decision
Izbrali smo **Nuxt + SQLite + Drizzle**, ker z enim jezikom in enim projektom pokrijemo frontend, API in PWA, kar je za dvočlansko ekipo najhitrejše. Obseg podatkov (manjše knjižnice) je majhen, zato SQLite zadošča. LLM ponudnik je **OpenAI** (structured outputs in tool calling); podrobnosti v kasnejšem ADR.

## Consequences
- Lažje: ena koda, skupni tipi med API-jem in frontendom, enostaven lokalni zagon brez baze kot storitve.
- Težje: v oblaku potrebujemo trajni disk za SQLite datoteko ali prehod na libSQL (Turso) — Drizzle podpira oboje, zato menjava ne zahteva prepisovanja poizvedb.
- Paziti moramo, da poslovna logika ostane v `server/services`, ne v route handlerjih ali Vue komponentah.
- Mobilni odjemalec je PWA; če predmet zahteva Android APK, ga zapakiramo s Trusted Web Activity (glej tveganja v [proposal.md](../proposal.md)).
