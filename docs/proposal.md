# Predlog projekta — Knjigomat

**Knjigomat** je informacijski sistem za upravljanje manjše knjižnice (splošne, šolske ali fakultetne). Člani v njem iščejo po katalogu, si izposojajo in rezervirajo knjige ter upravljajo svoje izposoje prek spleta ali telefona. Knjižničarji v sistemu vodijo knjižnično zbirko in izposojo.

## Problem
Številne manjše knjižnice (šolske, fakultetne, krajevne enote) izposojo še vedno vodijo v preglednicah, na papirnatih karticah ali v zastareli namizni programski opremi. Posledice:

- **Člani** ne morejo enostavno preveriti, ali je knjiga res na polici, ne vedo, kdaj bo rezervirana knjiga na voljo, in pozabljajo na roke vračila, kar vodi v zamudnine in slabo voljo.
- **Knjižničarji** veliko časa porabijo za rutinsko delo: preverjanje razpoložljivosti, ročno podaljševanje izposoj, opominjanje zamudnikov in računanje zamudnin.
- **Knjižnica** nima jasnega pregleda nad tem, kaj je izposojeno, kaj zamuja in po čem je povpraševanje, zato se o nabavi odloča na pamet.

Knjigomat združi katalog, izposojo, pravično čakalno vrsto za rezervacije ter samodejno upravljanje rokov in zamudnin v enem sistemu. Člani ga lahko uporabljajo s telefonom kar pred knjižno polico.

## Uporabniki in vloge
| Vloga | Opis | Kaj lahko počne |
|-------|------|-----------------|
| Član | Registriran uporabnik knjižnice (študent, občan …) | Išče po katalogu, vidi razpoložljivost, rezervira knjige, pregleduje in podaljšuje svoje izposoje, vidi svoje zamudnine, uporablja mobilno aplikacijo in AI asistenta |
| Knjižničar | Zaposleni na izposojevalnem pultu | Upravlja knjige in fizične izvode, izposoja in sprejema vrnjene izvode, upravlja rezervacije, odpisuje ali poravna zamudnine, pregleduje poročila o zamudah |
| Administrator | Skrbnik sistema | Upravlja uporabniške račune in vloge, nastavlja pravila izposoje (trajanje izposoje, največje število izposoj, največje število podaljšanj, dnevna zamudnina) |

Avtorizacija se izvaja na strežniku glede na vlogo **in** lastništvo: član lahko vidi in spreminja samo svoje izposoje, rezervacije in zamudnine.

## Uporabniške zgodbe

Oblika: *Kot <vloga> želim <cilj>, da <korist>.* Prioriteta (MoSCoW): **M** — must (nujno), **S** — should (zaželeno), **C** — could (lahko), **W** — won't (ne v tej različici).

| # | Zgodba | Prioriteta | Mejnik | Issue |
|---|--------|------------|--------|-------|
| 1 | Kot **član** želim iskati po katalogu po naslovu, avtorju, ISBN ali žanru in videti, koliko izvodov je na voljo, da vem, ali si knjigo lahko izposodim takoj. | M | vmesni zagovor | #… |
| 2 | Kot **knjižničar** želim dodajati, urejati in odstranjevati knjige ter njihove fizične izvode, da se katalog ujema s stanjem na policah. | M | vmesni zagovor | #… |
| 3 | Kot **knjižničar** želim izvod izposoditi članu in ga ob vrnitvi sprejeti nazaj, da je vsaka izposoja zabeležena z rokom vračila. | M | vmesni zagovor | #… |
| 4 | Kot **član** želim videti svoje trenutne izposoje in njihove roke vračila, da knjige vrnem pravočasno. | M | vmesni zagovor | #… |
| 5 | Kot **član** želim rezervirati knjigo, ki trenutno ni na voljo, da jo dobim takoj, ko je kakšen izvod vrnjen. | M | vmesni zagovor | #… |
| 6 | Kot **član** želim izposojo podaljšati prek spleta, da lahko knjigo obdržim dlje brez obiska knjižnice. | S | vmesni zagovor | #… |
| 7 | Kot **knjižničar** želim videti seznam izposoj z zamudo in dolgovanimi zamudninami, da lahko opomnim člane. | S | vmesni zagovor | #… |
| 8 | Kot **administrator** želim upravljati uporabniške račune in vloge ter nastavljati pravila izposoje, da lahko knjižnica sistem prilagodi svojim pravilom. | S | vmesni zagovor | #… |
| 9 | Kot **član** želim s telefonom skenirati črtno kodo ISBN na knjigi, da takoj vidim njeno razpoložljivost in jo lahko rezerviram. | M | končni zagovor | #… |
| 10 | Kot **član** želim z naravnim jezikom opisati, kaj iščem (npr. »nekaj o grafih za začetnike«), da najdem ustrezne knjige, tudi če ne poznam točnih naslovov. | M | končni zagovor | #… |
| 11 | Kot **član** želim AI asistenta, ki po moji potrditvi poišče knjigo, jo rezervira in podaljša moje izposoje, da lahko vse opravim v enem pogovoru. | M | končni zagovor | #… |
| 12 | Kot **član** želim prejeti obvestilo, ko je moja rezervirana knjiga pripravljena ali ko se izteka rok izposoje, da ne zamudim prevzema ali roka vračila. | C | končni zagovor | #… |
| 13 | Kot **član** želim zamudnine plačati prek spleta, da mi ni treba plačevati na pultu. | W | — | — |

### Kriteriji sprejemljivosti

**Zgodba 1 — iskanje po katalogu**
- **Če** katalog vsebuje knjigo »Uvod v algoritme« s 3 izvodi, od katerih je 1 izposojen,
- **ko** član išče »algoritmi«,
- **potem** se knjiga prikaže med rezultati z oznako *na voljo 2 od 3*.

**Zgodba 3 — izposoja in vračilo**
- **Če** ima član manj kot največje dovoljeno število izposoj in nima neporavnanih zamudnin nad dovoljeno mejo,
- **ko** mu knjižničar izposodi razpoložljiv izvod,
- **potem** se ustvari izposoja z rokom vračila *danes + trajanje izposoje*, izvod pa se označi kot izposojen.
- **Ko** je izvod vrnjen po roku, **potem** se članu zaračuna zamudnina v višini *število dni zamude × dnevna zamudnina*.

**Zgodba 5 — čakalna vrsta za rezervacije**
- **Če** so vsi izvodi knjige izposojeni,
- **ko** jo član rezervira,
- **potem** je dodan na konec čakalne vrste za to knjigo in vidi svoje mesto v vrsti.
- **Ko** je izvod vrnjen, **potem** se 3 dni hrani za prvega člana v vrsti in si ga v tem času nihče drug ne more izposoditi.

**Zgodba 6 — podaljšanje**
- **Če** je bila članova izposoja podaljšana manjkrat, kot je največje dovoljeno število podaljšanj,
- **ko** jo član podaljša **in** na to knjigo ne čaka noben drug član,
- **potem** se rok vračila podaljša za eno obdobje izposoje.
- **Sicer** se podaljšanje zavrne z jasnim razlogom (dosežena omejitev / knjigo je rezerviral nekdo drug / izposoja je že v zamudi).

**Zgodba 11 — AI agent**
- **Če** prijavljeni član vpraša: »Poišči mi razpoložljivo knjigo o podatkovnih bazah, jo rezerviraj in podaljšaj moje trenutne izposoje«,
- **ko** agent prek API-ja poišče knjige in izposoje,
- **potem** prikaže načrtovana dejanja (katero knjigo bo rezerviral, katere izposoje bo podaljšal) in **ne izvede ničesar**, dokler član ne potrdi.
- Vsa dejanja se izvajajo s članovimi lastnimi pravicami, zato podaljšanje, ki ga pravila ne dovoljujejo, zavrne in pojasni.

## Uporaba mobilne aplikacije
Telefon se uporablja **pred knjižno polico in na poti**:

- **Skeniranje črtne kode ISBN** s kamero: takoj prikaže razpoložljivost, lokacijo na polici in čakalno vrsto, knjigo pa je mogoče rezervirati z enim dotikom.
- **»Moje izposoje«** na poti: roki vračila, podaljšanje z enim dotikom in stanje rezervacij.
- **Obvestila** (zaželeno): ko je rezervirana knjiga pripravljena za prevzem ali ko se izteka rok izposoje.

To je smiselno ravno na telefonu: uporabnik stoji pred polico ali pa ni za računalnikom, ko se izteka rok vračila.

## AI funkcionalnosti
- **Funkcionalnost z LLM: iskanje po katalogu v naravnem jeziku.** Član vpiše prosto besedilo (»lahek uvod v strojno učenje v slovenščini«). LLM ga pretvori v strukturiran filter (tema/žanr, ključne besede, jezik, zahtevnost), ki se izvede na našem iskalnem endpointu. Rezultati so razvrščeni, vsak pa ima enovrstično razlago, zakaj ustreza iskanju.
- **Naloga agenta:** *»Poišči razpoložljivo knjigo na temo, ki jo opišem, jo rezerviraj in podaljšaj moje izposoje, ki se jim izteka rok — po moji potrditvi.«* Agent uporablja naš API (iskanje po katalogu → preverjanje razpoložljivosti → seznam mojih izposoj → rezervacija → podaljšanje). Dejanja, ki spreminjajo podatke, zahtevajo izrecno potrditev uporabnika, kar je uveljavljeno v kodi, vsak klic pa se izvede z žetonom prijavljenega člana.
