# DRG-Assist — Gullstandard testcase

Testcase for validering av [DRG-Assist](https://drgassist.com), eit AI-støtta beslutningsstøtteverktøy for medisinsk DRG-koding i norsk spesialisthelseteneste.

## Kva er dette?

23 syntetiske ortopediske case brukt til å evaluere om DRG-Assist foreslår korrekte prosedyrekodar (NCSP), diagnosar (ICD-10) og DRG-grupper i samsvar med ISF 2026-regelverket og Helsedirektoratet si kodeveiledning.

**Alle testcase er syntetiske** — generell medisinsk terminologi utan identifiserbar pasientinformasjon.

## Resultat

| Metric | Resultat |
|--------|----------|
| Prosedyre-nøyaktigheit | 23/23 (100 %) |
| Hovuddiagnose-nøyaktigheit | 23/23 (100 %) |
| DRG-nøyaktigheit | 23/23 (100 %) |

Sist køyrt: 11. mars 2026.

## Kvifor dette betyr noko

DRG-gruppa eit sjukehusopphald hamnar i avgjer refusjonen sjukehuset får. To mekanismar gjer at opphaldet kan vere *underkoda* — og verktøyet fangar begge.

## Eksempel 1: Tilleggstilstandar (CC-differanse)

Mange pasientar har tilleggstilstandar (t.d. diabetes, delirium, nyresvikt) som gjer behandlinga meir kompleks. Desse skal kodast som bidiagnosar og kan flytte opphaldet til ein høgare DRG-gruppe (CC = *complication/comorbidity*). I praksis vert dei ofte utelatne fordi klinikaren fokuserer på hovudinngrepet.

Klinikaren skriv ein vanleg klinisk beskriving — verktøyet tolkar friteksten, identifiserer kodar og tilleggstilstandar, og viser DRG-konsekvensen i sanntid:

### Bimalleolær ankelfraktur + diabetes type 2
- **Input:** «bimalleolær ankelfraktur operert med plate og skruer, pasienten har diabetes type 2»
- **Prosedyre:** NHJ62 — Osteosyntese fraktur i begge malleoler med plate og skruer
- **Hovuddiagnose:** S82.80 — Lukka bimalleolær fraktur
- **Bidiagnose med CC:** E11.9 — Diabetes mellitus type 2
- **DRG-effekt:** 219 (131 791 kr) → 218 (185 249 kr) = **+53 458 kr**

### Ryggbrudd L2–L4 + blæreparese
- **Input:** «ryggbrudd burst L2, L3 og L4, bakre fiksasjon med skruer og stag, nerveskade med blæreparese»
- **Prosedyre:** NAJ54 — Osteosyntese fraktur i lumbalkolumna med skruer og stag
- **Hovuddiagnose:** S32.00 — Lukka brudd i lumbalvirvel
- **Bidiagnose med CC:** G83.4 — Cauda equina-syndrom
- **DRG-effekt:** 215B (178 903 kr) → 214B (335 851 kr) = **+156 948 kr**

### Pertrokantær femurfraktur + postoperativt delirium
- **Input:** «pertrokantær femurfraktur hos 85 år gamal mann, operert med margnagle, postoperativt delirium»
- **Prosedyre:** NFJ51 — Osteosyntese pertrokantær femurfraktur med margnagle
- **Hovuddiagnose:** S72.10 — Lukka pertrokantært brudd
- **Bidiagnose med CC:** F05.9 — Delirium
- **DRG-effekt:** 211N (95 629 kr) → 210N (145 605 kr) = **+49 976 kr**

## Eksempel 2: Multitraume-deteksjon

Den andre mekanismen har endå større utslag. Når ein pasient har frakturar i to ulike kroppsregionar (t.d. arm + lår), skal hovuddiagnosen vere ein multitraumekode (T02.x) i staden for ein einskild frakturkode. Dette flyttar heile opphaldet til ei eiga multitraume-DRG-gruppe — men mønsteret er lett å oversjå når kirurgen kodar kvar fraktur for seg.

### Skaftfraktur humerus + femur — begge med margnagle
- **Input:** «fall frå stillas, open skaftfraktur humerus og lukka femurskaftfraktur, begge operert med margnagle»
- **Prosedyrar:** NBJ55 + NFJ54 — Osteosyntese med margnagle i humerus og femur
- **Hovuddiagnose utan multitraumekoding:** S42.31 — Ope brudd i humerusskaft
- **Hovuddiagnose med multitraumekoding:** T02.6 — Frakturar i overekstremitet og underekstremitet

| Koding | DRG | Refusjon |
|--------|-----|----------|
| Berre S-kodar (S42.31 som HD) | 219 | 131 791 kr |
| Korrekt T02.6 (multitraume) | 486 | 479 827 kr |
| **Differanse** | | **+348 036 kr** |

Verktøyet detekterer multitraume-mønsteret automatisk og foreslår T02.x som hovuddiagnose. Éin pasient, same behandling — men korrekt koding gjev nesten **350 000 kr meir** i refusjon.

## Eksempel 3: Reoperasjon — korrekt prosedyrekode

Ein tredje mekanisme: ved reoperasjon for komplikasjon etter tidlegare inngrep (infeksjon, blødning, sårruptur) skal prosedyren kodast med ein reopkode (W-kode) fyrst, etterfølgd av det spesifikke inngrepet (Kodeveiledningen kap. 7.3). I tillegg skal fjerning av osteosyntesemateriale kodast med NxU49 — ikkje den generelle NxU99. Utan dette hamnar opphaldet i feil DRG.

### Infisert margnagle tibia — fjerning + debridement — diabetes + KOLS
- **Input:** «infeksjon i osteosyntesemateriale i tibia 3 månader etter margnagling, diabetes type 2 og KOLS, fjerning av nagle og debridement»
- **Prosedyrar:** NGW69 (reoperasjon for dyp infeksjon) + NGU49 (fjerning av osteosyntesemateriale)
- **Hovuddiagnose:** T84.6 — Infeksjon i innvendig fiksasjonsanordning
- **Bidiagnosar med CC:** E11.9 (diabetes) + J44.9 (KOLS)

| Koding | DRG | Refusjon |
|--------|-----|----------|
| Utan W-kode, feil U-kode (NGU99) | 231 | 73 841 kr |
| Korrekt NGW69 + NGU49, med CC | 218 | 185 249 kr |
| **Differanse** | | **+111 408 kr** |

Verktøyet legg til W-koden automatisk når hovuddiagnosen er ein komplikasjonskode (T84/T81), og oppgraderer den generelle fjerningskoden til den spesifikke.

## Kategoriar

Casane dekker:
- Traumatiske frakturar (ankel, hofte, rygg, tibia, humerus, radius, femur, acetabulum)
- Patologiske frakturar (metastase, osteoporose)
- Multitraume (T02-kodar, DRG 486)
- Elektive inngrep (artroskopi, rotator cuff, planovalgus)
- Protesekirurgi (invers skulderprotese)
- Reoperasjon for komplikasjon (infeksjon, implantatfjerning)
- Postoperative komplikasjonar (nerveskade, karskade, compartment, DVT, delirium, blødning)

## Filformat

`golden.json` inneheld case-ID, klinisk beskriving og forventa kodar:

```json
{
  "id": 25,
  "name": "Bimalleolær ankelfraktur — plate/skruer — diabetes type 2",
  "text": "bimalleolær ankelfraktur operert med plate og skruer, pasienten har diabetes type 2",
  "expect": {
    "procs": ["NHJ62"],
    "hd": "S82",
    "drg": ["218", "219"]
  }
}
```

## Kontakt

Tarjei Vinje — [DRG Assist AS](https://drgassist.com)

## Lisens

CC BY 4.0 — Fri bruk med namngjeving.
