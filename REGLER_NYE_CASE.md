# Reglar for å leggje til nye testcase

## Masterfil

**`/Users/tarjeivinje/Desktop/DRG/v2_production_gcp/golden.json`** er einaste kjeldefil (single source of truth).

Etter endring i master skal GitHub-kopien (`/Users/tarjeivinje/Desktop/DRG/drg-assist-testcases/golden.json`) oppdaterast manuelt.

## ID-nummerering

- Case er nummererte **fortløpande frå 1**.
- Nytt case får `id` = noverande tal case + 1 (dvs. alltid siste i lista).
- Aldri hopp i nummereringa. Om eit case vert sletta, renummerer alle etterfølgjande.

## Format for kvart case

```json
{
  "id": 27,
  "name": "Kort, deskriptivt namn — hovudinngrep — evt. komplikasjon",
  "text": "klinisk fritekst slik ein klinikar ville skrive det, på nynorsk/bokmål",
  "expect": {
    "procs": ["NCSP-kode"],
    "hd": "ICD10-kode utan punktum",
    "drg": ["DRG1", "DRG2"]
  }
}
```

### Feltvariasjonar

| Felt | Bruk | Forklaring |
|------|------|------------|
| `procs` | Alle må matche | Bruk når det er éin eller fleire obligatoriske prosedyrar |
| `procs_any` | Minst éin må matche | Bruk når fleire prosedyrar er akseptable (t.d. fasciotomi kan kodast ulikt) |
| `hd` | Alltid påkravd | Hovuddiagnose, ICD-10 utan punktum. Kan vere prefiks (t.d. `"S82"` matchar `S82.0`, `S82.8` osv.) |
| `diags_contain` | Valfritt | Liste med ICD-kodar som skal finnast blant alle diagnosar (hovud + bi) |
| `mt_fractures` | Berre multitraume | Frakturkodar som skal vere til stades som bidiagnosar ved T02.x |
| `drg` | Alltid påkravd | Streng (`"486"`) eller liste (`["218", "219"]`) av akseptable DRG-grupper |

## Kvalitetskrav

1. **Verifiser NCSP-koden** — slå opp i NCSP.csv eller kodelista. LLM-ar hallusinerer prosedyrekodar.
2. **Verifiser ICD-10-koden** — sjekk at koden finst og er korrekt for tilstanden.
3. **Verifiser DRG-gruppa** — bruk appen (drgassist.com eller localhost) til å køyre casen og stadfeste at DRG stemmer.
4. **Ikkje legg til case du ikkje har køyrt gjennom appen.** Testcasa skal vere validerte, ikkje teoretiske.
5. **Unngå duplikat** — sjekk at det ikkje allereie finst eit case med same inngrep og diagnose.

## Kjende datamanglar i appen

### Korsbånd-kodar (NGE60-77) manglar i app-data

**Problem**: 12 korsbånd-kodar finst i `proc.csv` (har PROCPR-gruppering) men manglar i `NCSP.csv` (har ikkje norsk tekst). Build-scriptet (`build_drg_app.py` linje 957) krev tekst frå NCSP.csv og filtrerer vekk kodar utan tekst.

**Kodar som manglar i appen:**
- NGE60-63: Primær rekonstruksjon (fremre/bakre korsbånd, mediale/laterale kollateral)
- NGE70-77: Revisjon av tidlegare rekonstruert korsbånd

**Kodar som er utgåtte** (finst i NCSP.csv men IKKJE i proc.csv):
- NGE41-56: Gamle rekonstruksjonskodar (u/m protesemateriale) — ikkje lenger grupperte

**Konsekvens**: Appen foreslår NGE35 (transposisjon) for ACL-rekonstruksjon i staden for NGE60 (primær rekonstruksjon). Ikkje lag testcase for korsbåndskirurgi før dette er fiksa.

**FIX**: Build-scriptet må hente tekst frå `proc.csv` som fallback når `NCSP.csv` manglar tekst, eller `NCSP.csv` må oppdaterast med dei nye kodane. Kjelde: `proc.csv` linje 25223-25240 (NGE60-63), og tilsvarande for NGE70-77.

## Kva case er nyttige å leggje til?

Prioriter case som viser ein av desse mekanismane:

- **CC-differanse**: Bidiagnose (diabetes, delirium, nyresvikt osv.) som endrar DRG-par
- **Multitraume**: Frakturar i 2+ grovregionar → T02.x → DRG 486
- **Reoperasjon**: W-kode + spesifikk U-kode ved komplikasjon
- **Nye anatomiske regionar** som ikkje er dekte (sjå kategoriar i TESTCASES.md)
- **Elektive inngrep** — protesekirurgi, artroskopi, ryggkirurgi

Unngå å leggje til mange case som testar same mekanisme og same anatomi.

## Oppdatering av andre filer

Etter at golden.json (master) er oppdatert:

1. **Kopier til GitHub-mappa**: `cp v2_production_gcp/golden.json drg-assist-testcases/golden.json`
2. **Oppdater TESTCASES.md**: Legg til ny rad i tabellen og oppdater tal i overskrift/resultat
3. **Push til GitHub**: `cd drg-assist-testcases && git add -A && git commit && git push`
