# Predlog projekta — Shelfie

**Shelfie** je informacijski sistem za upravljanje manjših, neformalnih knjižnic v študentskih domovih, društvih in klubih. Člani prek spleta ali telefona iščejo po katalogu ter si izposojajo, rezervirajo in podaljšujejo knjige. Skrbniki (običajno prostovoljci) v njem vodijo zbirko in izposojo.

## Problem
Študentski domovi, društva in klubi imajo pogosto svoje manjše knjižnice, ki jih vodijo prostovoljci v preglednicah ali pa sploh ne, zato knjige izginjajo in nihče ne ve, kdo ima kaj. Skrbnik ni stalno prisoten, zato člani ne vedo, ali je knjiga na voljo, rezervacije pa potekajo »na besedo«.

**Zakaj obstoječa orodja tega ne rešijo:** COBISS je namenjen pravim knjižnicam z zaposlenimi knjižničarji in je za takšne zbirke predrag in prezapleten. Preglednice nimajo vlog, rokov, čakalnih vrst ali opomnikov, aplikacije za osebne knjižne police (npr. Goodreads) pa ne podpirajo izposoje med člani. Shelfie zapolni to vrzel: enostavno vodenje izposoje, rezervacij in rokov vračila, člani pa do knjig dostopajo tudi s telefonom.

## Uporabniki in vloge
En uporabnik je lahko član več knjižnic (npr. doma in društva), **vloga pa velja za posamezno knjižnico**.

| Vloga | Opis | Kaj lahko počne |
|-------|------|-----------------|
| Član | Stanovalec doma ali član društva/kluba | Išče po katalogu, vidi razpoložljivost, rezervira knjige, pregleduje in podaljšuje **svoje** izposoje, uporablja mobilno aplikacijo in AI asistenta |
| Skrbnik | Prostovoljec, ki skrbi za knjižnico | Vse kot član + upravlja knjige in izvode, izposoja in sprejema vrnjene izvode, upravlja rezervacije, vidi izposoje z zamudo, blokira/odblokira člane |
| Administrator | Upravitelj knjižnice (npr. predsednik društva) | Vse kot skrbnik + upravlja članstva in vloge, nastavlja pravila izposoje (trajanje, največ izposoj, največ podaljšanj, rok prevzema rezervacije) |

### Pravila avtorizacije (preverja strežnik)
| Dejanje | Član | Skrbnik | Admin |
|---------|:----:|:-------:|:-----:|
| Iskanje po katalogu svoje knjižnice | ✓ | ✓ | ✓ |
| Rezervacija, ogled in podaljšanje **lastnih** izposoj | ✓ | ✓ | ✓ |
| Ogled izposoj drugih članov | ✗ | ✓ | ✓ |
| Dodajanje/urejanje knjig in izvodov | ✗ | ✓ | ✓ |
| Izposoja in vračilo izvoda | ✗ | ✓ | ✓ |
| Blokiranje člana | ✗ | ✓ | ✓ |
| Upravljanje članstev, vlog in pravil | ✗ | ✗ | ✓ |

Vsak dostop je omejen na knjižnice, katerih član je uporabnik (preverjanje vloge **in** lastništva).

## Uporabniške zgodbe

Oblika: *Kot <vloga> želim <cilj>, da <korist>.* Prioriteta (MoSCoW): **M** — must, **S** — should, **C** — could, **W** — won't (ne v tej različici).

| # | Zgodba | Prioriteta | Mejnik | Issue |
|---|--------|------------|--------|-------|
| 1 | Kot **član** želim iskati po katalogu po naslovu, avtorju, ISBN ali žanru in videti, koliko izvodov je na voljo, da vem, ali si knjigo lahko izposodim takoj. | M | midterm | #2 |
| 2 | Kot **skrbnik** želim dodajati, urejati in odstranjevati knjige ter njihove izvode, da se katalog ujema s stanjem na polici. | M | midterm | #3 |
| 3 | Kot **skrbnik** želim izvod izposoditi članu in ga ob vrnitvi sprejeti nazaj, da je vsaka izposoja zabeležena z rokom vračila. | M | midterm | #4 |
| 4 | Kot **član** želim videti svoje trenutne izposoje in njihove roke vračila, da knjige vrnem pravočasno. | M | midterm | #5 |
| 5 | Kot **član** želim rezervirati knjigo, ki trenutno ni na voljo, da jo dobim takoj, ko je kakšen izvod vrnjen. | M | midterm | #6 |
| 6 | Kot **član** želim izposojo podaljšati prek spleta, da lahko knjigo obdržim dlje, ne da bi iskal skrbnika. | S | midterm | #7 |
| 7 | Kot **skrbnik** želim videti seznam izposoj z zamudo, da lahko opomnim člane in dobim knjige nazaj. | S | midterm | #8 |
| 8 | Kot **administrator** želim upravljati članstva, vloge in pravila izposoje, da knjižnico prilagodimo svojim pravilom. | S | midterm | #9 |
| 9 | Kot **član** želim s telefonom skenirati črtno kodo ISBN na knjigi, da takoj vidim njeno razpoložljivost in jo lahko rezerviram. | M | final | #10 |
| 10 | Kot **član** želim z naravnim jezikom opisati, kaj iščem (npr. »nekaj o grafih za začetnike«), da najdem ustrezne knjige, tudi če ne poznam točnih naslovov. | M | final | #11 |
| 11 | Kot **član** želim AI asistenta, ki po moji potrditvi poišče knjigo, jo rezervira in podaljša moje izposoje, da lahko vse opravim v enem pogovoru. | M | final | #12 |
| 12 | Kot **član** želim prejeti obvestilo, ko je moja rezervirana knjiga pripravljena ali ko se izteka rok izposoje, da ne zamudim prevzema ali roka vračila. | C | final | #13 |
| 13 | Kot **skrbnik** želim zbirko uvoziti iz preglednice (CSV), da mi obstoječih knjig ni treba vnašati ročno. | C | final | #14 |

**Won't (zavestno izven obsega):**
- Povezava s COBISS — ciljamo zbirke, ki niso v COBISS.
- Zamudnine in spletna plačila — prostovoljne knjižnice ne pobirajo denarja; namesto tega blokada izposoje ob zamudi.
- Medknjižnična izposoja (knjiga iz ene knjižnice v drugo).

### Kriteriji sprejemljivosti (Must)

**1 — iskanje po katalogu**
- **Če** katalog vsebuje »Uvod v algoritme« s 3 izvodi, od katerih je 1 izposojen, **ko** član išče »algoritmi«, **potem** se knjiga prikaže z oznako *na voljo 2 od 3*.
- *Neuspešna pot:* **če** ni zadetkov, **potem** sistem to jasno sporoči in predlaga iskanje v naravnem jeziku.

**2 — upravljanje zbirke**
- **Če** je uporabnik skrbnik, **ko** doda knjigo z ISBN in 2 izvodoma, **potem** je knjiga v katalogu s 2 razpoložljivima izvodoma.
- *Neuspešna pot:* izvoda, ki je izposojen, ni mogoče odstraniti; član, ki pokliče ta endpoint, dobi 403.

**3 — izposoja in vračilo**
- **Če** ima član manj kot največ dovoljenih izposoj in nobene zamude, **ko** mu skrbnik izposodi razpoložljiv izvod, **potem** se ustvari izposoja z rokom *danes + trajanje izposoje* in izvod je označen kot izposojen.
- **Ko** je izvod vrnjen in nekdo čaka v vrsti, **potem** izvod preide v stanje *na prevzemu* za prvega v vrsti.
- *Neuspešna pot:* **če** ima član izposojo z zamudo, je blokiran ali je dosegel omejitev, **potem** se izposoja zavrne z razlogom.

**4 — moje izposoje**
- **Če** ima član 2 aktivni izposoji, **ko** odpre »Moje izposoje«, **potem** vidi obe z rokom vračila, zamujene so označene.
- *Neuspešna pot:* zahteva za izposoje drugega člana vrne 403.

**5 — rezervacija in čakalna vrsta**
- **Če** so vsi izvodi izposojeni, **ko** član rezervira knjigo, **potem** je dodan na konec vrste in vidi svoje mesto.
- **Ko** je izvod vrnjen, se hrani za prvega v vrsti *N* dni (pravilo knjižnice); če ga ne prevzame, rezervacija poteče in izvod gre naslednjemu.
- *Neuspešna pot:* rezervacija se zavrne, če je izvod na voljo, če član knjigo že ima ali je že v vrsti.

**9 — skeniranje ISBN (mobilno)**
- **Če** je knjiga v katalogu, **ko** član skenira njeno črtno kodo, **potem** se odpre stran knjige z razpoložljivostjo in gumbom za rezervacijo.
- *Neuspešna pot:* **če** ISBN ni v katalogu, sistem to sporoči; **če** dostop do kamere ni dovoljen, lahko član ISBN vpiše ročno.

**10 — iskanje v naravnem jeziku**
- **Ko** član vpiše »lahek uvod v strojno učenje v slovenščini«, **potem** LLM vrne strukturiran filter (JSON po shemi), iskanje se izvede na našem API-ju, vsak zadetek ima kratko razlago.
- *Neuspešna pot:* **če** je odgovor LLM neveljaven ali storitev ni dosegljiva, sistem preklopi na navadno iskanje po ključnih besedah.

**11 — AI agent**
- **Ko** član reče »Poišči razpoložljivo knjigo o podatkovnih bazah, jo rezerviraj in podaljšaj moje izposoje«, **potem** agent prek API-ja zbere podatke in prikaže načrt (katera rezervacija, katera podaljšanja) ter **ne izvede ničesar pred potrditvijo**.
- *Neuspešna pot:* **če** član zavrne, se nič ne spremeni; **če** pravila podaljšanje prepovedujejo, API to zavrne, agent pa razlog pojasni.

## Uporaba mobilne aplikacije
Mobilni odjemalec bo **progresivna spletna aplikacija (PWA)**, ki si jo uporabnik namesti na začetni zaslon telefona in uporablja isti API kot spletna aplikacija. Telefon se uporablja **pred knjižno polico in na poti**, kar je pomembno, ker v neformalnih knjižnicah skrbnik pogosto ni prisoten:

- **Skeniranje črtne kode ISBN** s kamero: takoj prikaže razpoložljivost in čakalno vrsto, knjigo je mogoče rezervirati z enim dotikom.
- **»Moje izposoje«** na poti: roki vračila, podaljšanje z enim dotikom, stanje rezervacij; zadnje stanje je na voljo tudi brez povezave.
- **Potisna obvestila** (could): rezervirana knjiga je pripravljena, rok izposoje se izteka.

## AI funkcionalnosti
- **LLM s strukturiranim izhodom: iskanje v naravnem jeziku.** Prosto besedilo člana se pošlje OpenAI modelu, ki vrne JSON po vnaprej določeni shemi (`{ keywords[], genre?, language?, level? }`). Strežnik JSON validira in ga pretvori v poizvedbo na naš iskalni endpoint; ob neveljavnem izhodu sledi navadno iskanje.
- **Naloga agenta:** *»Poišči razpoložljivo knjigo na temo, ki jo opišem, jo rezerviraj in podaljšaj moje izposoje, ki se jim izteka rok — po moji potrditvi.«* Koraki: iskanje po katalogu → preverjanje razpoložljivosti → seznam mojih izposoj → **točka potrditve** (prikaz načrta) → rezervacija → podaljšanja. Orodja kličejo naš API s sejo prijavljenega člana, potrditev pred pisalnimi orodji je uveljavljena v kodi, število korakov je omejeno, vsak korak se beleži. Podrobnosti v [agent.md](agent.md).

## Načrt in tveganja

**Midterm (30. 11. – 4. 12. 2026):** spletna aplikacija + baza: zgodbe 1–5 (Must) ter 6–8 (Should), prijava in vloge, osnovna postavitev v oblak.
**Final (11. – 15. 1. 2027):** PWA s skeniranjem (9), LLM iskanje (10), agent (11), po možnosti 12–13; dokončan API in objava v oblaku.

| Tveganje | Načrt |
|----------|-------|
| PWA morda ne zadošča zahtevi »odjemalec Android« | Pri asistentu preverimo do midterma; po potrebi PWA zapakiramo v APK s Trusted Web Activity (Bubblewrap), koda ostane ista. |
| SQLite v oblaku (ena datoteka, en zapisovalec) | Gostovanje s trajnim diskom; za večje obremenitve prehod na libSQL/Turso brez menjave ORM (Drizzle). Glej [0001-stack.md](decisions/0001-stack.md). |
| Nezanesljiv izhod LLM | Validacija sheme, rezervno navadno iskanje, potrditev pred vsakim pisanjem. |
| Logika čakalne vrste je zahtevna (sočasne vrnitve in rezervacije) | Logika v servisnem sloju, transakcije, testi za robne primere pred midtermom. |
| Obseg za 2 osebi v 9 tednih | Must zgodbe najprej; Could (12–13) samo, če ostane čas. |
