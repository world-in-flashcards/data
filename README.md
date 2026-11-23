# World in Flashcards: data

The master tables behind the [World in Flashcards](https://github.com/world-in-flashcards/decks) decks: every
value a card shows that a person or a reading decided, and why. One row per region, keyed on its ISO 3166 code (or
a GADM code where ISO has none).

These are **the master tables**: the deck generator reads them from here, and every deck version is built from a
commit of this repository. Corrections are welcome as issues or pull requests (see [Reporting an
error](#reporting-an-error)).

## The tables

Each file is one of four kinds: **decided** (a value or ruling a person or a reading made, with its reason; edit it
to change the deck), **measured** (written by a generator tool from every card's rendered map; regenerated, never
edited by hand), **reference** (a published list kept as given) or **bookkeeping** (the state of the work, which no
card shows).

**What a card says**

| file | kind | holds |
|---|---|---|
| `notes.tsv` | decided | each card's hand-entered fields (English and native names, capital, IPA, captions, languages, status, category, codes, currency, calling code, domain, capital coordinates) and `#why`, the reason for each value |
| `entities.tsv` | decided | the stable identity of every card: project key, code, the note GUID Anki uses, live or defunct |
| `code_history.tsv` | decided | every card code ever changed: old and new code, date, reason, ruling, evidence |
| `gadm_iso.tsv` | decided | which GADM unit each ISO-keyed card is drawn from, and the basis for the key (identifiers only) |
| `defunct.tsv` | decided | units a boundary dataset still draws that no longer exist as drawn: their last values, successors and source; kept out of the deck |
| `capital_points.tsv` | decided | each capital city's point, the source it was chosen from by rule R1, or the open question about it |
| `audio.tsv`, `audio_eng.tsv` | decided | the one recording chosen for each half of a native or English place or capital name: the Commons file, its source, licence and author |
| `types.tsv` | decided | what each unit type is called in English, and the qualifier a card adds when two answers collide |
| `type_words.tsv` | decided | the same type words in the country's own language, in their inflected forms |
| `m49.tsv` | reference | UN M49 as the UN Statistics Division publishes it: each country's region, for the continent tags |
| `m49_placements.tsv`, `m49_subdivisions.tsv` | decided | cards M49 does not list, placed by an M49 country or region; subdivisions on another continent than their country |
| `landlocked.tsv`, `un_nsgt.tsv` | reference | the landlocked and doubly landlocked countries, and the UN's non-self-governing territories, from the Wikipedia lists cited, joined on card codes: two feature tags |

**Which flags and emblems a card gets**

| file | kind | holds |
|---|---|---|
| `choices.tsv` | decided | the file chosen for a card's flag or emblem slot, and who chose it |
| `rejected.tsv` | decided | files refused for a slot, so no later search brings them back |
| `no_image.tsv` | decided | rulings that a unit has no flag or emblem of its own, with the evidence and its tier |
| `masks.tsv` | decided | the boxes of lettering painted out of a picture so its question does not name the answer, keyed on the file's SHA-256 |
| `mask_review.tsv` | decided | a person's or reader's verdict on each masked picture as drawn: `clean`, `name-legible`, `damaged` or `kept-unmasked` |
| `fotw_links.tsv` | decided | a reader's link from a card to its page on FOTW Flags Of The World, where no code joins them |
| `hotw_links.tsv` | decided | the same for Heraldry of the World; empty, as the site could not be read |
| `geosiwk_hand.tsv` | decided | the ruling on each note of the model deck (Geography Stuff I Wanna Know) that no name could match to a card |

**What the boundary datasets are taken to say**

| file | kind | holds |
|---|---|---|
| `dataset_codes.tsv` | decided | which card code a dataset's unit takes where the dataset carries none, or carries a defective one: Overture's divisions, COD-AB's P-codes, the World Bank's codes, geoBoundaries' damaged codes, Natural Earth's units refiled or merged |
| `unit_items.tsv` | decided | the Wikidata item a unit is, where no dataset joins it; and the items refused as some other place |
| `level0_units.tsv` | decided | the country-level cards with no ISO code or no plain dataset row: the Antarctic claims, the de facto states, Rockall, Null Island and the rest, each with the dataset unit it is drawn from and a point it must cover |
| `claimant_units.tsv` | decided | the subdivision cards a claimant state draws over ground another state's card holds, each with how its ground is drawn |
| `reorganised_units.tsv` | decided | units a reorganisation created that no dataset draws yet, carved or cut from the units that do |
| `held_ground.tsv` | decided | ground drawn as one card's though its dataset files it elsewhere or nowhere: disputed areas that are whole cards, exclaves, land a release dropped |
| `geometry_repairs.tsv` | decided | defects in GADM, geoBoundaries and Natural Earth geometry and how each is mended - ground filed under the wrong unit, swapped rows, missing islands - each guarded by points from another source |
| `text_repairs.tsv` | decided | defects in GADM's text - names cut at 32 characters, misspelt type words, native names joined without a separator - and which half of a two-script native name is kept |
| `dataset_types.tsv` | decided | the category a dataset's type word, or a country's whole dataset, gives its units |
| `source_terms.tsv` | reference | the source and licence each geoBoundaries country file states, and GADM's exception for Austria |

**How the maps are drawn**

| file | kind | holds |
|---|---|---|
| `disputed_areas.tsv` | decided | each disputed area: who holds it, who claims it, and how the cards draw it |
| `disputed_units.tsv` | decided | the subdivision cards that are their country's claiming unit for a disputed area; `acts_for` names the claimant a unit acts for where that is not its own country (Western Sahara's provinces, for Morocco, east of the Berm) |
| `disputed_seas.tsv` | decided | which card draws each overlapping or jointly administered sea zone (Marine Regions ids) |
| `subdivision_land.tsv`, `subdivision_trim.tsv` | decided | countries whose subdivisions are cut to Natural Earth's land, or to the country card's own outline |
| `subdivision_water.tsv` | decided | subdivision cards drawn less a lagoon or bay their dataset draws as land |
| `subdivision_overture_land.tsv` | decided | atoll countries whose units are their region's whole area cut to Overture's land |
| `added_lakes.tsv` | decided | lakes Natural Earth omits, added from Overture, and the cards that draw them |
| `panel_water.tsv`, `panel_zoom.tsv` | decided | detail panels drawn with Overture's water, or with a closer zoom and Overture's coast (a floor for every card too small to read with no useful panel, and Overture's reefs for the atolls) |
| `disputed_cards.tsv`, `ice_shelf_cards.tsv`, `outlying_cards.tsv`, `panel_ring_cards.tsv`, `frame_clip_cards.tsv`, `frame_ring_cards.tsv`, `overlay_above_cards.tsv`, `locator_floor_cards.tsv`, `zone_hole_countries.tsv`, `gap_lake_countries.tsv`, `claimed_ground_countries.tsv`, `odd_tile_maps.tsv`, `weak_panel_cards.tsv`, `archipelago_sea_countries.tsv` | measured | the cards (or countries, or question maps) a map rule changes - disputed areas and border lines, Antarctica's ice shelves, outlying parts and the locator, detail-panel rings, cut panel frames, the ring drawn where a detail frame would be smaller than it or the panel no finer than the map, lines above the highlight redrawn on a shared map's overlay, the locator widened to at least 1.5 times the main map, zone holes filled as internal water, parent-gap rows a lake covers whole, ground a country's units draw but it does not hold drawn grey on their siblings, the question maps where a card odd one out on its tile moves, detail panels declined as no finer than 1.5 times the map and ringing nothing, and the archipelagos whose subdivision cards tint their country's sea; the list decides which cards the rule applies to |

**The work itself**

| file | kind | holds |
|---|---|---|
| `questions.tsv` | bookkeeping | open and settled questions on a card's field, with the candidates and the verdict |
| `research.tsv` | bookkeeping | rows a reader flagged for a deeper pass, or as a unit no longer current |
| `imported.tsv` | bookkeeping | every card field an import or a rule fill wrote; the coverage report counts these as imported, not checked |
| `status.toml` | bookkeeping | how complete each country's fields are declared to be, per field group |
| `pcgn_bad_rows.tsv` | bookkeeping | rows of the PCGN Toponymic Factfiles known to be wrong (an error in the factfile or in reading it), which the generator skips |
| `unit_types.tsv` | bookkeeping | retired (2026-09-19) and frozen: the early research on what English calls each country's unit types; a card's `category` cell now decides |

All `.tsv` files are tab-separated UTF-8 with a byte-order mark, one row per line; `status.toml` is TOML.

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
  not included (https://gadm.org/).
- **ISO 3166** - codes as identifiers.
- **Geography Stuff I Wanna Know** by AlmePond (https://github.com/AlmePond/Geography_Stuff_I_Wanna_Know) - the
  Anki deck that inspired this one and served as its model: its note schema and its design. It is also a minor data
  source: the first values were imported from it, and it is credited on the few values that still rest on it alone
  (some pronunciations and a capital name that no other stored source gives), each citing the deck in its reasons.
  <!-- D10: ruled 2026-11-16 by Jens Nordmark (@nordmarj) -->


## Reporting an error

Open an issue naming the card's code, the field, what it says, what it should say, and a source.
