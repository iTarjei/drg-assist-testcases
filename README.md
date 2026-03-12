# DRG-Assist — Testcase og syntetiske journalar

Eksempel frå [DRG-Assist](https://drgassist.com), eit AI-støtta beslutningsstøtteverktøy for medisinsk DRG-koding i norsk spesialisthelseteneste.

## Kva er dette?

3 utvalde eksempel med **syntetisk klinisk journal** og **gullstandard-kodar** (NCSP, ICD-10, DRG). Journalane er generert frå kliniske case-spesifikasjonar — ingen reelle pasientdata.

Verktøyet er validert mot 49 ortopediske testcase med 100 % nøyaktigheit (prosedyre, hovuddiagnose, DRG).

## Eksempel

### `examples/`

| Case | Journal | Viser |
|------|---------|-------|
| 20: Ankelfraktur + diabetes | `journal_case20.txt` | CC-split (+53 458 kr) |
| 28: Hoftefraktur + underernæring + delirium | `journal_case28.txt` | Major CC, skjulte skattar |
| 6: Multitraume bilulykke | `journal_case06.txt` | Multitraumedeteksjon |

`examples.json` inneheld case-spesifikasjon og forventa kodar for kvart case.

## Kvifor dette betyr noko

DRG-gruppa eit sjukehusopphald hamnar i avgjer refusjonen sjukehuset får. Tre mekanismar gjer at opphaldet kan vere *underkoda* — og verktøyet fangar alle.

## Mekanisme 1: Tilleggstilstandar (CC-differanse)

Mange pasientar har tilleggstilstandar (t.d. diabetes, delirium, nyresvikt) som gjer behandlinga meir kompleks. Desse skal kodast som bidiagnosar og kan flytte opphaldet til ein høgare DRG-gruppe (CC = *complication/comorbidity*). I praksis vert dei ofte utelatne fordi klinikaren fokuserer på hovudinngrepet.

Klinikaren skriv ein vanleg klinisk beskriving — verktøyet tolkar friteksten, identifiserer kodar og tilleggstilstandar, og viser DRG-konsekvensen i sanntid:

### Bimalleolær ankelfraktur + diabetes type 2 (Case 20)
- **Input:** «bimalleolær ankelfraktur operert med plate og skruer, pasienten har diabetes type 2»
- **Prosedyre:** NHJ62 — Osteosyntese fraktur i begge malleoler med plate og skruer
- **Hovuddiagnose:** S82.80 — Lukka bimalleolær fraktur
- **Bidiagnose med CC:** E11.9 — Diabetes mellitus type 2
- **DRG-effekt:** 219 (131 791 kr) → 218 (185 249 kr) = **+53 458 kr**

### Pertrokantær femurfraktur + underernæring + delirium (Case 28)
- **Input:** «pertrokantær femurfraktur, 82 år, operert med margnagle, kjend underernæring, postoperativt delirium»
- **Prosedyre:** NFJ51 — Osteosyntese pertrokantær femurfraktur med margnagle
- **Hovuddiagnose:** S72.10 — Lukka pertrokantært brudd
- **Bidiagnosar med CC:** E46 (underernæring, major CC) + F05.9 (delirium)
- **DRG-effekt:** 211N (95 629 kr) → 210N (145 605 kr) = **+49 976 kr**

## Mekanisme 2: Multitraume-deteksjon

Når ein pasient har frakturar i to ulike kroppsregionar (t.d. arm + lår), skal hovuddiagnosen vere ein multitraumekode (T02.x). Dette flyttar heile opphaldet til ei eiga multitraume-DRG-gruppe — men mønsteret er lett å oversjå.

### Bilulykke — skaftfraktur humerus + femur (Case 6)
- **Input:** «bilulykke, open skaftfraktur femur og lukka humerusfraktur, operert med margnagle begge, arterieskade, compartmentsyndrom»
- **Prosedyrar:** NFJ54 + NBJ55 + NGM09
- **Hovuddiagnose:** T02.6 — Frakturar i overekstremitet og underekstremitet

| Koding | DRG | Refusjon |
|--------|-----|----------|
| Berre S-kodar (S42.31 som HD) | 219 | 131 791 kr |
| Korrekt T02.6 (multitraume) | 486 | 479 827 kr |
| **Differanse** | | **+348 036 kr** |

Verktøyet detekterer multitraume-mønsteret automatisk og foreslår T02.x som hovuddiagnose.

## Filformat

`examples/examples.json` inneheld case-ID, klinisk beskriving og forventa kodar:

```json
{
  "case_id": 20,
  "name": "Bimalleolær ankelfraktur — plate/skruer — diabetes type 2",
  "golden_expect": {
    "procs": ["NHJ62"],
    "hd": "S82",
    "diags_contain": ["S82", "E119"],
    "drg": ["218", "219"]
  }
}
```

Kvar case har ein tilhøyrande syntetisk journal (`examples/journal_caseXX.txt`).

## Kontakt

Tarjei Vinje — [DRG Assist AS](https://drgassist.com)

## Lisens

CC BY 4.0 — Fri bruk med namngjeving.
