# Testcase — oversikt

26 syntetiske ortopediske case. Kvar rad viser klinisk beskriving (input), forventa prosedyrekode (NCSP), hovuddiagnose (ICD-10) og DRG-gruppe. Alle case er validerte mot ISF 2026-regelverket.

| # | Klinisk beskriving | Prosedyre (NCSP) | Hovuddiagnose (ICD-10) | DRG |
|---|---|---|---|---|
| 1 | Proksimal femurfraktur, metastase ca mammae, sementert hemiprotese | NFB12 | C79.5 | 209 |
| 2 | Humerusskaftfraktur, metastase ca coli, nagle | NBJ55 | C79.5 | 218/219 |
| 3 | Distal humerusfraktur, plate og skruer, postop nerveskade n. radialis | NBJ66 | S42.4 | 218/219 |
| 4 | Acetabulumfraktur, plate og skruer, nerveskade n. ischiadicus med dropfot | NEJ69 | S32.4 | 210/211 |
| 5 | Distal femurfraktur, plate/skruer/cerclage, arterieskade, compartmentsyndrom | NFJ65 | S72.4 | 210/211 |
| 6 | Bilulykke, multitraume humerus+femur, arterieskade, compartmentsyndrom | NFJ54, NBJ55 | T02.6 | 486 |
| 7 | Planovalgus fot, calcaneus-forlenging, subtalar artrodese, cotton-osteotomi | NHK65, NHG41 | M21.4 | 218/219 |
| 8 | Burst-fraktur L2–L4, bakre fiksasjon med skruer og stag, blæreparese | NAJ54 | S32.0 | 214/215 |
| 9 | Tibialplatåfraktur, plate og skruer, atrieflimmer, postop blødning | NGJ61 | S82.1 | 218/219 |
| 10 | Fallulykke, multitraume humerus+femur, margnagle begge | NBJ55, NFJ54 | T02.6 | 486 |
| 11 | Open tibiaskaftfraktur, stor blautdelsskade, margnagle, delhudstransplantat | NGJ52 | S82.2 | 217 |
| 12 | Lukka tibiaskaftfraktur, margnagle | NGJ52 | S82.2 | 218/219 |
| 13 | MC-ulykke, multitraume ribbeinsbrudd+femur, hemopneumothorax, margnagle | NFJ54 | T02.7 | 486 |
| 14 | Distal radiusfraktur, volar plate, kjend osteoporose | NCJ65 | S52.5 | 223/224 |
| 15 | C2-fraktur, bakre fiksasjon med skruer og stag | NAJ50 | S12.1 | 214/215 |
| 16 | Artroskopisk meniskektomi, gammal medial meniskruptur | NGD01 | M23.2 | 221/222/232 |
| 17 | Open tibiaskaftfraktur, MC-ulykke, margnagle, postop DVT | NGJ52 | S82.2 | 218/219 |
| 18 | Torakalvirtelfraktur, inkomplett ryggmargskade, bakre fiksasjon | NAJ52 | S22.0 | 214B/215B |
| 19 | Pertrokantær femurfraktur, 85 år, margnagle, postop delirium | NFJ51 | S72.1 | 210/211 |
| 20 | Bimalleolær ankelfraktur, plate og skruer, diabetes type 2 | NHJ62 | S82.8 | 218/219 |
| 21 | Proksimal humerusfraktur, invers skulderprotese hybrid, diabetes type 2 | NBB30 | S42.2 | 491 |
| 22 | Artroskopisk rotator cuff-reparasjon, kronisk ruptur | NBL49 | M75.1 | 232 |
| 23 | Infisert margnagle tibia, fjerning+debridement, diabetes+KOLS | NGW69, NGU49 | T84.6 | 218/219 |
| 24 | Hallux valgus, osteotomi 1. metatars, skruefiksasjon | NHK57 | M20.1 | 225 |
| 25 | Spondylolistese L4/L5, bakre spondylodese, diabetes type 2 | NAG74 | M43.1 | 214B/215B |
| 26 | Open trimalleolær ankelfraktur-luksasjon, plate/skruer, diabetes | NHJ63 | S82.8 | 218/219 |

## DRG-effekt: To typar kodingsmønster

Testcasa illustrerer to ulike mekanismar der korrekt koding endrar DRG-gruppe:

### 1. CC-differanse (bidiagnose endrar gruppe innanfor same DRG-par)

Ein tilleggstilstand (t.d. diabetes, delirium, blæreparese) gjev CC-status og flyttar frå «utan CC» til «med CC»-varianten av same DRG. Typisk differanse: **+50 000 – 157 000 kr**.

| Case | Bidiagnose | DRG-endring | Differanse |
|------|-----------|-------------|------------|
| #20 Ankelfraktur + diabetes | E11.9 | 219 → 218 | +53 458 kr |
| #8 Ryggbrudd + blæreparese | G83.4 | 215B → 214B | +156 948 kr |
| #19 Hoftefraktur + delirium | F05.9 | 211N → 210N | +49 976 kr |

### 2. Multitraume-deteksjon (kodingsmønster flyttar heile DRG-gruppa)

Når frakturar i to ulike grov-regionar (t.d. overekstremitet + underekstremitet) vert koda med T02.x i staden for separate S-kodar, byter grupperinga frå vanleg ortopedisk DRG til multitraume-DRG (MDC 24). Mykje større utslag.

| Case | Utan T02.x | Med T02.x | Differanse |
|------|-----------|-----------|------------|
| #10 Humerus + femur, margnagle begge | DRG 219 (131 791 kr) | DRG 486 (479 827 kr) | **+348 036 kr** |

### 3. Reoperasjon — korrekt prosedyrekode (W-kode + spesifikk U-kode)

Ved komplikasjon etter tidlegare inngrep skal prosedyren kodast med reopkode (W-kode) fyrst, og fjerning av osteosyntesemateriale med NxU49 (ikkje generell NxU99). Utan dette hamnar opphaldet i feil DRG.

| Case | Utan W-kode | Med NGW69 + NGU49 + CC | Differanse |
|------|------------|------------------------|------------|
| #23 Infisert margnagle tibia + diabetes + KOLS | DRG 231 (73 841 kr) | DRG 218 (185 249 kr) | **+111 408 kr** |

## Kategoriar

Casane dekker:
- **Traumatiske frakturar** — ankel, hofte, rygg, tibia, humerus, radius, femur, acetabulum
- **Patologiske frakturar** — metastase, osteoporose
- **Multitraume** — T02-kodar, DRG 486
- **Elektive inngrep** — artroskopi, rotator cuff, planovalgus
- **Protesekirurgi** — invers skulderprotese
- **Komplikasjonar** — nerveskade, karskade, compartment, DVT, delirium, blødning

## Resultat

| Metric | Resultat |
|--------|----------|
| Prosedyre-nøyaktigheit | 26/26 (100 %) |
| Hovuddiagnose-nøyaktigheit | 26/26 (100 %) |
| DRG-nøyaktigheit | 26/26 (100 %) |

Sist køyrt: 11. mars 2026.
