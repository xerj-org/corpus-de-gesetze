# corpus-de-gesetze

German federal statutes as plain text, one file per Norm, for retrieval /
reference-corpus use. Mirror of the official **gesetze-im-internet.de**
publications (Bundesministerium der Justiz / Bundesamt für Justiz).

## Scope

The five federal statutes an agent working today is actually asked about,
chosen demand-side (not by legal-system completeness):

| Law | Abkürzung | Files | Norms in source | Skipped (no text) |
|---|---|---:|---:|---:|
| Grundgesetz für die Bundesrepublik Deutschland | GG | 201 | 220 | 19 |
| Bürgerliches Gesetzbuch | BGB | 2519 | 2841 | 322 |
| Strafgesetzbuch | StGB | 547 | 630 | 83 |
| Straßenverkehrs-Ordnung | StVO | 64 | 68 | 4 |
| Verwaltungsgerichtsordnung | VwGO | 215 | 240 | 25 |
| **Total** | | **3546** | **3999** | **453** |

"Skipped (no text)" norms are structural headings (Buch/Abschnitt/Gliederungseinheiten)
and repealed-with-no-content markers (e.g. `(XXXX) §§ 207 bis 210 (weggefallen)`)
which carry no norm text; they are not mirrored as files.

EU-law layer (DSGVO) and its national implementation (BDSG) are deliberately
**out of scope** for this corpus: DSGVO is an EU regulation published on
eur-lex.europa.eu, a different source lane; BDSG would be a sixth law outside
the five chosen. If needed it can be added the same way (`bdsg/xml.zip`).

## Source and download lane

- Source: <https://www.gesetze-im-internet.de/>, official XML zip per law:
  `https://www.gesetze-im-internet.de/<kürzel>/xml.zip`
  (GG: `gg`, BGB: `bgb`, StGB: `stgb`, VwGO: `vwgo`, StVO: `stvo_2013` —
  StVO lives under its konstitutive-Neufassung key).
- Downloaded: **2026-10-04** (curl, direct zip download; all five zips verified
  200 before extraction).
- Consolidation status per law, taken from the `standangabe` in the XML itself
  (dates are the dates of the amending instruments, not of this mirror):

  | Law | Ausfertigung | Fundstelle | Last change recorded in XML |
  |---|---|---|---|
  | GG | 1949-05-23 | BGBl 1949, 1 | Art. 1 G v. 22.3.2025 I Nr. 94 |
  | BGB | 1896-08-18 | RGBl 1896, 195 | Art. 6 G v. 23.7.2026 I Nr. 226 |
  | StGB | 1871-05-15 | RGBl 1871, 127 | Art. 1 G v. 20.3.2026 I Nr. 95 |
  | StVO | 2013-03-06 | BGBl I 2013, 367 | Art. 4 V v. 30.1.2026 I Nr. 32 |
  | VwGO | 1960-01-21 | BGBl I 1960, 17 | Art. 12a G v. 23.4.2026 I Nr. 111 |

## File format

One plain-text file per Norm with substantive text: `<law>/<enbez>-<ABK>.txt`,
e.g. `bgb/§-249-BGB.txt`, `gg/Art-1-GG.txt`, `stvo/Anlage-1-StVO.txt`,
`stgb/Eingangsformel-StGB.txt`.

Each file:

1. Header lines: law (langue + Abkürzung), Norm (enbez + titel), XML-Stand
   (builddate of that norm block), dokumentnummer, per-norm HTML page on
   gesetze-im-internet.de, source zip URL, licence line.
2. Empty line, then the Norm text **verbatim, UTF-8**. Absatz numbers are part
   of the paragraph text as published (`(1) …`, `(2) …`). Inline enumerations
   keep their numbers (`1. … 2. …`).

Rendering decisions (recorded so downstream users know what is lossy):

- `<fussnoten>` / `<Footnotes>` (editorial hints:Amtliche Hinweise,
  Textnachweis-`(+++ … +++)` notes, BVerfGE status notes) are **excluded**.
- Tables (StVO Anlagen) are rendered one row per line, cells joined with
  ` | `.
- Traffic-sign graphics in the StVO Anlagen are **not** shipped (this is a
  text corpus); their positions are marked `[Bild: <dateiname.jpg>]`
  referencing the file names inside the official `stvo_2013/xml.zip`.
- § 211 StGB's Mordmerkmale list appears as separate lines without letters —
  that is exactly how gesetze-im-internet encodes it (no letters in the XML).

## MANIFEST

`MANIFEST.tsv` — columns: `path`, `source_zip` (the exact zip the bytes came
from), `norm_html` (per-norm HTML page on gesetze-im-internet.de),
`doknr` (official document number of the norm block).

The `norm_html` URLs are constructed from the enbez pattern
(`§ N` → `__N.html`, `Art N` → `art_N.html`, `Anlage N` → `anlage_N.html`,
unnumbered → law index). Verified 200 on 2026-10-04 for a sample across all
laws and pattern types: `bgb/__249.html`, `bgb/__240a.html`, `stgb/__211.html`,
`vwgo/__40.html`, `gg/art_1.html`, `gg/art_104b.html`, `stvo_2013/__1.html`,
`stvo_2013/__8.html`, `stvo_2013/anlage_1.html`.

## Licence

Public domain — no copyright attaches.

- German copyright law, **§ 5 UrhG**: "Gesetze, Verordnungen, amtliche Erlasse
  und Bekanntmachungen sowie Entscheidungen und amtlich verfaßte Leitsätze zu
  Entscheidungen genießen keinen urheberrechtlichen Schutz." These are
  statutes published by the federal government.
- The publisher's own reuse statement on the gesetze-im-internet.de start page
  (verified 2026-10-04, <https://www.gesetze-im-internet.de/>): "Die
  Rechtsnormen in deutscher Sprache stehen in allen angebotenen Formaten zur
  freien Nutzung und Weiterverwendung zur Verfügung."
- Impressum (<https://www.gesetze-im-internet.de/impressum.html>): publisher is
  the Bundesrepublik Deutschland (Bundesministerium der Justiz), editorial and
  technical provision by Bundesamt für Justiz / juris GmbH.

## Reproduction

```sh
for z in gg bgb stgb vwgo stvo_2013; do
  curl -O "https://www.gesetze-im-internet.de/$z/xml.zip"
done
# then: one file per <norm> with non-empty <textdaten><text><Content>
```

Mirror built 2026-10-04 for the XERJ corpus hub.
