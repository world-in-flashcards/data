# World in Flashcards: data

The master tables behind the [World in Flashcards](https://github.com/world-in-flashcards/decks) decks: every
value a card shows that a person or a reading decided, and why. One row per region, keyed on its ISO 3166 code (or
a GADM code where ISO has none).

These are **the master tables**: the deck generator reads them from here, and every deck version is built from a
commit of this repository. Corrections are welcome as issues or pull requests (see [Reporting an
error](#reporting-an-error)).

## The tables

| file | holds |
|---|---|
| `notes.tsv` | each card's hand-entered fields (English and native names, capital, IPA, captions, languages, status, codes, currency, calling code, domain, capital coordinates) and `#why`, the reason for each value |
| `entities.tsv` | the stable identity of every card: project key, code, the note GUID Anki uses, live or defunct |
| `code_history.tsv` | every card code ever changed, with the ruling |
| `gadm_iso.tsv` | which GADM unit each ISO-keyed card is drawn from (identifiers only) |
| `capital_points.tsv` | each capital city's point and the source it was chosen from |
| `questions.tsv`, `research.tsv` | open and settled questions, and rows waiting for a deeper pass |
| `no_image.tsv`, `rejected.tsv`, `choices.tsv` | rulings on flags and emblems: none exists, a file refused, a file chosen |
| `types.tsv`, `type_words.tsv` | what each unit type is called in English, and in its country's language |
| `m49*.tsv` | the UN M49 regions used for the continent tags, and the placements M49 does not give |
| `disputed_*.tsv` | who holds and who claims each disputed area |
| the rest | the cards a map rule applies to, measured |

All files are tab-separated UTF-8 with a byte-order mark, one row per line.

### Reading `#why`

Each `#why` cell holds one entry per field, `field: reason`, separated by `;;`. A reason says what the value is,
which sources say so and what each gave, and the rule that decided it. Rules are cited by name and date, such as
*"the none-found convention of 2026-09-30"* or *"the Saba ruling"*. **[RULES.md](RULES.md) holds every rule the
tables follow**, and its index lists every name a cell uses.

A reason names its sources by cite:

- `wiki:en:266098@1371056481` - English Wikipedia, page id 266098, revision 1371056481
  (`https://en.wikipedia.org/w/index.php?oldid=1371056481`);
- `web:<16 hex>` - a stored snapshot of a web page; the page's URL usually follows it;
- `geonames:<id>`, `overture:<id>` - a GeoNames feature, an Overture Maps feature;
- `data:<16 hex>` - the deck generator's read of the boundary datasets named for that row, as compared on a given
  day: reproducible from those datasets, and not itself published.

Text in backticks after a cite is a quotation from that source.

## Quotations

- Quotations of pages that are not openly licensed (government sites, news and other websites, FOTW Flags Of The
  World) are cut to their first 25 words, marked `…`; the full text is in the stored snapshot the cite names.
- `#name` is a row label for people reading the file (often GADM's name for the unit); no build reads it.

## Licence

The tables as a database are under the **Open Database License (ODbL) 1.0**, and their contents under **CC BY-SA
4.0** (see [LICENSE](LICENSE)), Copyright (c) 2026 Jens Nordmark. In short: you may copy, change and build on them,
including commercially, if you credit this project and the sources below and share what you make from them under
the same terms.

Not covered: quotations of pages that are not openly licensed (government sites, news, other websites) and of
FOTW Flags Of The World's text are included as quotation, each with its source named; they are not ours to
license. FOTW's text may be reused only unaltered and for non-commercial, non-political purposes
(https://www.crwflags.com/fotw/flags/disclaim.html).

## Sources and their terms

Each value's `#why` names its sources. Reuse of the tables must carry these attributions:

- **Wikipedia and Wiktionary** - quotations from the revisions cited, by their contributors, under CC BY-SA 4.0.
- **OpenStreetMap**, via the Overture Maps Foundation - capital points (`capital_points.tsv` rows sourced
  `overture`, `capitalCoord` values citing `overture:`) and names: © OpenStreetMap contributors, ODbL 1.0,
  https://www.openstreetmap.org/copyright.
- **GeoNames** - capital points and names: GeoNames (https://www.geonames.org/), CC BY 4.0.
- **PCGN Toponymic Factfiles** - Contains public sector information licensed under the Open Government Licence
  v3.0 (https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/).
- **OCHA Common Operational Datasets (COD-AB)**, via HDX - CC BY-IGO.
- **World Bank Official Boundaries** - CC BY 4.0.
- **geoBoundaries** - names from each country's file, under that file's licence (CC BY, CC BY-SA, ODbL or public
  domain), https://www.geoboundaries.org/.
- **Marine Regions**, Maritime Boundaries v12 - zone ids and names in `disputed_seas.tsv`, CC BY 4.0.
- **UN M49**, the Standard country or area codes for statistical use, by the United Nations Statistics Division -
  `m49.tsv`, the regions and codes as the Division publishes them, kept as a table of facts
  (https://unstats.un.org/unsd/methodology/m49/overview/); the UN's terms of use govern its website
  (https://www.un.org/en/about-us/terms-of-use). <!-- D8: ruled 2026-11-16 by Jens Nordmark (@nordmarj) -->
- **Wikidata** - CC0. **Natural Earth** - public domain.
- **GADM** - unit codes (GID_1) as keys, and a few names and type words quoted as evidence; GADM's data itself is
  not included (https://gadm.org/, free for academic and other non-commercial use).
- **ISO 3166** - codes as identifiers.
- **Geography Stuff I Wanna Know** by AlmePond (https://github.com/AlmePond/Geography_Stuff_I_Wanna_Know) - the
  Anki deck that inspired this one and served as its model: its note schema and its design. It is also a minor data
  source: the first values were imported from it, and it is credited on the few values that still rest on it alone
  (some pronunciations and a capital name that no other stored source gives), each citing the deck in its reasons.
  <!-- D10: ruled 2026-11-16 by Jens Nordmark (@nordmarj) -->


## Reporting an error

Open an issue naming the card's code, the field, what it says, what it should say, and a source.
