# DRG-Assist — Gullstandard testcase

Testcase for validering av [DRG-Assist](https://drgassist.com), eit AI-støtta beslutningsstøtteverktøy for medisinsk DRG-koding i norsk spesialisthelseteneste.

## Kva er dette?

22 syntetiske ortopediske case brukt til å evaluere om DRG-Assist foreslår korrekte prosedyrekodar (NCSP), diagnosar (ICD-10) og DRG-grupper i samsvar med ISF 2026-regelverket og Helsedirektoratet si kodeveiledning.

**Alle testcase er syntetiske** — generell medisinsk terminologi utan identifiserbar pasientinformasjon.

## Resultat

| Metric | Resultat |
|--------|----------|
| Prosedyre-nøyaktigheit | 22/22 (100 %) |
| Hovuddiagnose-nøyaktigheit | 22/22 (100 %) |
| DRG-nøyaktigheit | 22/22 (100 %) |

Sist køyrt: 11. mars 2026.

## Eksempel: CC-differanse

Klinikaren skriv ein vanleg klinisk beskriving — verktøyet tolkar friteksten, identifiserer prosedyrekodar, diagnosar og tilleggstilstandar, og viser DRG-konsekvensen i sanntid. Tre eksempel på korleis ein bidiagnose endrar DRG-gruppe:

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

## Eksempel: Multitraume-deteksjon

CC-differanse viser effekten av tilleggstilstandar *innanfor* ei DRG-gruppe. Men nokre kodingsmønster flyttar heile grupperinga til ein høgare DRG — med langt større utslag.

Når ein pasient har frakturar i to ulike grov-regionar (t.d. overekstremitet + underekstremitet), skal hovuddiagnosen vere ein T02.x-kode (multitraume) i staden for ein einskild S-kode. Dette flyttar grupperinga frå vanleg ortopedisk DRG til multitraume-DRG (MDC 24).

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

Verktøyet detekterer multitraume-mønsteret automatisk og foreslår T02.x som hovuddiagnose — noko ein klinikar lett kan oversjå når frakturane er koda kvar for seg.

## Kategoriar

Casane dekker:
- Traumatiske frakturar (ankel, hofte, rygg, tibia, humerus, radius, femur, acetabulum)
- Patologiske frakturar (metastase, osteoporose)
- Multitraume (T02-kodar, DRG 486)
- Elektive inngrep (artroskopi, rotator cuff, planovalgus)
- Protesekirurgi (invers skulderprotese)
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
