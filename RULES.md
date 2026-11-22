# World in Flashcards: the content rules

These are the rules the tables in this repository follow: what each field holds, which source decides, and what
happens where sources disagree or say nothing. A table cell names the rule it follows by its name and date, for
example *"the none-found convention of 2026-09-30"* or *"the Saba ruling"*. This file is where to look the rule
up. [The index of rule names](#index-of-rule-names) at the end lists every name the cells use, and [the
index by date](#index-by-date) lists every date.

**Who ruled.** Every ruling here was made by the project's owner, Jens Nordmark (@nordmarj). Rulings from
before 2026-09-17 were not yet signed in the tables; they are attributed to him here too. A rule called a
*convention* was not ruled on a case. It is how a field is written, and it applies without asking.

**Overturned rules.** Only the current rule is given. Where a cell may still name the rule it replaced, one
clause says what that rule was and when it was overturned.

**Examples.** Where a rule names a card (Saba, Kyiv, Potosí), the card is an example of the rule. The ruling on
that one card is in that card's `#why`.

## Contents

1. [Reading the tables](#reading-the-tables)
2. [Principles that cut across fields](#principles)
3. [Identity: codes, keys and GUIDs](#identity)
4. [The note fields](#fields)
5. [Names](#names)
6. [Languages](#languages)
7. [Category and status](#category-and-status)
8. [Capitals](#capitals)
9. [Pronunciation (IPA)](#ipa)
10. [Audio](#audio)
11. [Registry fields: ISO codes, currency, calling code, ccTLD](#registry-fields)
12. [Captions, notes and `mem`](#captions-notes)
13. [Flags and emblems](#flags-and-emblems)
14. [Masking lettering that names the answer](#masking)
15. [Maps](#maps)
16. [Disputed areas and claims](#disputed)
17. [Defunct units](#defunct)
18. [Continents and tags](#tags)
19. [Sources and citing](#sources)
20. [Index of rule names](#index-of-rule-names)
21. [Index by date](#index-by-date)

---

<a id="reading-the-tables"></a>
## 1. Reading the tables

<a id="dash-sentinel"></a>
### `-`, an empty cell, and `#why`

- **`-` means deliberately blank**: someone decided the field has no value for this card. **An empty cell
  means no one has decided yet.** It is not an instruction to blank the field.
- **A non-empty cell wins. Where the cell is empty, the value derived from the datasets stands.**
- **`#why` gives the reason for each value, field by field.** It says what the value is, which sources say
  so (with their cites and quotes), what each source gave, the reasoning, the rule that decided it, any doubt,
  and how to redo the check (ruled 2026-11-17 by Jens Nordmark (@nordmarj)).
- **A value whose reason names a person is that person's ruling.** A later reading does not overwrite it. A
  reader who thinks it is wrong raises a question.
- **"By hand"** means a decision made by looking at the source or the picture, not by a script. **"By hand, by
  an agent"** means work an AI assistant did under these rules, which no person ruled on (ruled 2026-11-17 by
  Jens Nordmark (@nordmarj)).

<a id="verdicts"></a>
### Questions and their verdicts

`questions.tsv` holds one question per cell. Its `verdict` is one of these:

| verdict | meaning |
|---|---|
| empty | open |
| `resolved` | answered |
| `not-a-problem` | the sources differ, and our value is right as it stands |
| `no-answer` | no source can settle it |
| `deferred` | waits, still treated as open |
| `reject` | the picture is refused for this card, and recorded in `rejected.tsv` so it is never staged again |
| `replace` | the picture is replaced by the file the question names |
| `both` | both pictures belong on the card (a seal and a coat of arms) |

- **An open question holds back the value or file that would settle it** (2026-09-25). A person's
  `choices.tsv` row releases a held-back file.
- **A question may be closed where the value or file already there stands** (ruled 2026-09-30 by Jens
  Nordmark (@nordmarj)). The close quotes the page that shows it.
- **A `reject` outranks every choice.** A question that is no longer raised is kept and marked, never
  deleted.

---

<a id="principles"></a>
## 2. Principles that cut across fields

<a id="adjudicated"></a>
- **Every value is adjudicated** (2026-09-15). A cell holds a decided value, not a copy of one dataset. Its
  `#why` names the sources compared.
<a id="never-names"></a>
- **Nothing is keyed on a display name.** Joins between datasets are made on codes, on ids, or on geometry,
  never on a name. Names are ambiguous, contested, and they change.
<a id="no-authority"></a>
- **No dataset is an authority.** Each boundary dataset, Wikidata, GeoNames, Flags of the World and Heraldry
  of the World is evidence to weigh against the others. Each has measured defects. A dataset line never
  outranks a page that contradicts it.
<a id="silence"></a>
- **A silent source is not evidence.** Where a source has nothing, the data is missing. Only a source that
  *says* there is none counts as saying so.
<a id="seed"></a>
- **A seed is not laundered into evidence** (ruled 2026-09-20 by Jens Nordmark (@nordmarj)). A value
  imported unchecked says so ("NOT RESEARCHED", "nobody has checked it") and lists the raw words.
<a id="too-few"></a>
- **Too few is better than too many.** Where a rule sets a bar (a language, a capital, a picture), a value
  that does not clearly pass is left out.
<a id="file-outranks-ruling"></a>
- **A file always outranks a no-image ruling.** A ruling that a region has no flag stands only until a file
  is found.
<a id="does-more"></a>
- **Licence terms never cost function.** Where two sources are equally correct, the one that does more is
  taken (the deeper tier, the more current data), never the one with the freer licence. The exception is the
  pictures the deck ships, which must be shareable ([the licence ruling](#licence-ruling)).
<a id="no-special-case"></a>
- **No region is special-cased to make one card look right.** A card that seems to need a special case
  shows that the rule is wrong, so the rule is what gets changed.

---

<a id="identity"></a>
## 3. Identity: codes, keys and GUIDs

<a id="codes"></a>
### Card codes

- A country card is keyed on its **ISO 3166-1 alpha-3** code. Units without one take a minted code, such as
  `XTR` (Transnistria), `XQ*` (the Antarctic claims) or `XRK` (Rockall).
- A subdivision card is keyed on its **ISO 3166-2** code wherever the code is agreed by the datasets,
  confirmed by a reading, or ruled. Otherwise it keeps its **GADM `GID_1`** (ruled 2026-10-21 by Jens
  Nordmark (@nordmarj): "Cards move to ISO codes wherever possible"). `gadm_iso.tsv` records which GADM unit
  each ISO-keyed card is drawn from. The GID stays the join key to GADM.
- **A code that a current source uses before ISO does is used provisionally** (2026-09-14). This can be an
  official number or an invented code, and it is replaced by ISO's code once ISO assigns one.
- **A ruled code wins over the agreed key** (ruled 2026-09-30 and 2026-10-21 by Jens Nordmark (@nordmarj)).
  Bonaire, Saba and Sint Eustatius show `BQ-BO`, `BQ-SA` and `BQ-SE`; Svalbard shows `NO-21`. A unit whose
  ground a repair redrew keeps the register's code (`BS-SW`, `GN-K`, `GN-N`, `KW-KU`).

<a id="entity-keys"></a>
### Entity keys and note GUIDs

Ruled 2026-10-20 by Jens Nordmark (@nordmarj).

- `entities.tsv` gives every card an **opaque project key** (`T000001`, …; `P` is reserved for physical
  features), its current code, its **note GUID**, and its status (`live` or `defunct`). A key is never
  reused.
- **The GUID is frozen.** It is the value that the old derivation gave for the code on the day the key was
  minted, and it is never recomputed. Anki uses it to update a note instead of duplicating it. A card with no
  row in `entities.tsv` is refused.
- <a id="same-entity"></a>**A unit keeps its key while it continues.** A unit that is renamed, re-coded, has its
  boundary adjusted, or loses ground to a new unit keeps its key and its GUID. A new unit takes a new key. A
  unit that is dissolved, or split into equal parts, is retired, and its successors are new.
- `code_history.tsv` records every code change: the old code, the new code, the date, the kind of change, the
  ruling and the evidence.

<a id="geometry-rule"></a>
### The geometry rule (code changes)

Ruled 2026-10-22 by Jens Nordmark (@nordmarj), for the cases where no ruling said whether a former code's unit
continues. The old unit is compared with today's units:

- Several of today's units lie within it (each at least 90% inside): it was **split**.
- Exactly one unit lies within it: that unit **continues it** (re-coded), less the ground it lost.
- It lies wholly inside one unit, with at least 95% overlap both ways: **re-coded**. With less overlap:
  **merged**.
- It has no counterpart: **defunct**.

---

<a id="fields"></a>
## 4. The note fields

One note type, `Region (World in Flashcards)`. **New fields are appended, never inserted**, because Anki stores
a note's values as a positional list. **A card exists only where its source field is filled**, so a note with
only a map and a name gives one card.

| field | holds | written |
|---|---|---|
| `placeEng`, `placeOri` | the English and the native name ([Names](#names)) | in the table; else derived |
| `capitalEng`, `capitalOri` | the capital's English and native name ([Capitals](#capitals)) | in the table |
| `placeIPA`, `capitalIPA` | the pronunciation of the native names ([IPA](#ipa)) | in the table |
| `descFlag1..5`, `descEmblem1..2` | captions ([Captions](#captions)) | in the table; the level label is generated |
| `flag1..5`, `emblem1..2`, `…Full` | the pictures, masked on the question side and unmasked in `…Full` | generated from files |
| `map`, `questionMap`, `mapCredit` | the map, the map for the tap-the-region card, and its OpenStreetMap credit | generated |
| `note` | short context the answer needs ([The note field](#note-field)) | in the table |
| `language`, `otherLanguages` | the languages of the native names, and the region's other languages ([Languages](#languages)) | in the table |
| `category`, `status` | the unit type, and the political status ([Category and status](#category-and-status)) | in the table; else derived |
| `admin` | the parent country's card name: the same string as the parent card's `placeEng`, without its qualifier | generated from the parent code |
| `mem` | the learner's own field | never written by the project |
| `source` | the licence trail: map, pictures, audio, and the sources the values cite | generated |
| ISO code fields, `currency`, `callingCode`, `ccTld` | registry values ([Registry fields](#registry-fields)) | ISO codes generated; the rest in the table |
| `capitalCoord`, `capitalDot` | the capital's coordinate, and its dot on the map | the coordinate in the table; the dot generated |
| `placeAudio`, `capitalAudio`, `placeEngAudio`, `capitalEngAudio` | recordings of the names ([Audio](#audio)) | generated from `audio.tsv` and `audio_eng.tsv` |

- **The English name is always shown** on the card, never hidden behind a button (ruled 2026-10-19 by Jens
  Nordmark (@nordmarj)).
- **`category` is editable** (ruled 2026-09-18 by Jens Nordmark (@nordmarj)). The boundary datasets are
  sources like any other.

---

<a id="names"></a>
## 5. Names

<a id="naming-policy"></a>
### The naming policy

Settled 2026-09-07, with later rulings below. No source wins by rule: a disagreement is settled per value, in
`notes.tsv`.

<a id="title-rule"></a>
#### The English name: the title rule

Ruled 2026-09-30 by Jens Nordmark (@nordmarj); refined 2026-10-01.

- **`placeEng` carries the unit type where the English Wikipedia article title does.** Examples:
  `Kagoshima Prefecture`, `Chelyabinsk Oblast`, `Kebbi State`, and `Chukotka Autonomous Okrug`. The title
  decides, not the lead's opening words. The type then also appears in `category`, and that repetition is
  accepted.
- **A type word taken from a lower-case title is capitalised**: `Assaba Region` (ruled 2026-10-01 by Jens
  Nordmark (@nordmarj)).
- **The title decides the name itself too**: `Świętokrzyskie Voivodeship`, not Holy Cross (ruled 2026-10-01 by
  Jens Nordmark (@nordmarj)). The title's dashes are kept. Its disambiguator (`, Argentina`, `(Honduras)`) is
  never part of the name.
- **The title wins over the rule "Helmand, not Helmand Province"**, which once added a type word only as the
  country's other cards did.
- **The Gambia ruling** (2026-10-03, by Jens Nordmark (@nordmarj)): for the Gambia, the country's constitution
  and law outrank the title. The card is `North Bank Region` (the Local Government Act's name), not `North Bank
  Division` or `Kerewan`; the old name and the council go in `note`.

<a id="translate-transliterate"></a>
#### Translate what describes, transliterate what names

- A compass direction or other describing word is translated: `Kalimantan Timur` becomes `East Kalimantan`.
- A name is romanised, never replaced by an invented English form: `Gədəbəy` becomes `Gadabay`.
- **An established English exonym wins**: `Kyiv`, `Erbil`.

<a id="no-dataset-column"></a>
#### No dataset's English column is preferred wholesale

Natural Earth's `name_en` gives the same name to two units of one country more often than its local `name`
does. In Latvia, 21 municipalities share the `name_en` "Valmiera". So no source's English column wins by rule.

<a id="country-name-rule"></a>
#### A country card is named by its English Wikipedia title (the country-name rule)

Ruled 2026-09-14 by Jens Nordmark (@nordmarj).

- **A country card takes its English Wikipedia article title**: `China`, `United States`, `Timor-Leste`,
  `Cocos (Keeling) Islands`, `Northern Cyprus`.
- **The exception is a title that only disambiguates.** `Georgia (country)`, `Bailiwick of Guernsey`,
  `Republic of Ireland`, `Kingdom of the Netherlands` and `Collectivity of Saint Martin` keep our name.
- **The country card's name is the `admin` label of every subdivision**, so renaming a country relabels all of
  its cards.

<a id="western-sahara-ruling"></a>
#### A claimed state is not an alternative name (the Western Sahara ruling)

Ruled 2026-09-20 by Jens Nordmark (@nordmarj). A slash joins two names for **one** thing (`Corse / Corsica`).
It never joins a territory and a state that claims it. Western Sahara's card keeps its title, its `category`
and `status` read `disputed territory`, and the Sahrawi Arab Democratic Republic goes in `note`.

<a id="placeori-conventions"></a>
### The native name: the placeOri conventions

Conventions of 2026-09-15, as amended since.

- `placeOri` is the name **in the language and script of the place**: the country's script, and the
  language(s) that [the language rules](#languages) give.
- <a id="short-form"></a>**The short-form convention** (2026-09-15; **overturned in part on 2026-10-01**). A
  native name used to be written without its type word (`Шумен`, not `Област Шумен`). It still is, wherever
  `placeEng` carries no type word. Where `placeEng` carries one, [the type-word rule](#type-word-rule) now
  applies.
- <a id="bare-adjective"></a>**The bare-adjective convention** (2026-09-15) is **overturned**. It wrote Russia's
  oblasts as a bare adjective (`Челябинская`). See [the type-word rule](#type-word-rule).
- A native name identical to the English one is still written.

<a id="type-word-rule"></a>
#### The type-word rule

Ruled 2026-10-01 by Jens Nordmark (@nordmarj).

- **Wherever `placeEng` carries the unit type, `placeOri` carries it too.** It is written in the native
  language, in the form and word order of the region's official name: `Челябинская область`, `محافظة مسقط`,
  `Provincia de Coclé`, `कोशी प्रदेश`, `จังหวัดเชียงใหม่`. On a bilingual row, both halves carry it.
- This overturns the bare-adjective convention, the 2026-09-15 ruling that Thailand's native names drop
  `จังหวัด`, and the dropping of `محافظة` and `प्रदेश`.
- North Macedonia's municipalities take `Општина` (ruled 2026-10-28 by Jens Nordmark (@nordmarj)).
- <a id="tamazight-ruling"></a>**The Tamazight ruling** (2026-10-03, by Jens Nordmark (@nordmarj)): an Algerian
  province's Tamazight half takes `ⵜⴰⵡⵉⵍⴰⵢⵜ ⵏ` before the card's own Tamazight name, which is otherwise left
  unchanged: `ولاية أدرار / ⵜⴰⵡⵉⵍⴰⵢⵜ ⵏ ⴰⴷⵔⴰⵔ`.
- Mauritius's districts are `District de X`: the French form from running text, capitalised as a name (ruled
  2026-10-04 by Jens Nordmark (@nordmarj)).

<a id="montenegro-ruling"></a>
#### The Montenegro ruling

Ruled 2026-09-14 by Jens Nordmark (@nordmarj). Montenegro's native names are written in **Latin script**: the
country card (`Crna Gora`), the capitals, and the municipalities (`Opština Bar`). The same ruling drops `市` from
Chinese capitals' native names and `시` from North Korean ones; Taiwan keeps `市`, matching its English `City`.

<a id="local-language-name"></a>
#### Subdivisions use the language the country card uses

Ruled 2026-09-15. A subdivision's native name is in the local language that the country card uses, unless there
is a reason not to. Examples: Palauan (`Oreor`), Pijin (`Wes Provins`, keeping `Provins` as the sources write
it), Krio. A pronunciation of the old name is emptied with it.

<a id="ghana-convention"></a>
#### The Ghana convention

Convention of 2026-09-15, confirmed 2026-11-17 by Jens Nordmark (@nordmarj). In a country whose language is
English, `placeOri` is the English name, with its type word included: Ghana's `Oti Region`.

<a id="single-name-rulings"></a>
#### Rulings on single countries that set a convention

- <a id="stand-alone-convention"></a>**Sri Lanka, the stand-alone convention** (ruled 2026-09-26 by Jens Nordmark
  (@nordmarj)): every Sinhala `placeOri` takes the stand-alone form, ending in -ය where the name has one
  (අනුරාධපුරය), as the Election Commission lists the districts. The IPA follows that form. A Tamil spelling
  that changes a phoneme is judged case by case, by which form is the traditional one.
- **New Zealand** (ruled 2026-09-26 by Jens Nordmark (@nordmarj)): each region takes its own Māori name
  (Gisborne `Te Tairāwhiti`; Wellington `Te Upoko o te Ika a Māui`, not the city's name).
- **The Marshall Islands** (ruled 2026-09-26 by Jens Nordmark (@nordmarj)): the dictionary orthography,
  Marshallese only (`Jebwad`, `Ṃajeḷ`, `Mājro`).
- <a id="baikonur-ruling"></a>**The Baikonur ruling** (2026-09-21, by Jens Nordmark (@nordmarj)): a leased
  complex on another state's ground is named in the lessee's language, then the lessor's: `Космодром Байконур /
  Байқоңыр ғарыш айлағы`.

<a id="qualifiers"></a>
### Qualifiers

Settled 2026-09-07. A qualifier is derived, not typed.

- **Where two answers collide, a qualifier separates them**:

  | collision | separated by | example |
  |---|---|---|
  | a unit and its own capital | the type | `Kagoshima (prefecture)` |
  | two units of one country | the type | `Minsk (city)` / `Minsk (region)` |
  | units in different countries | the country | `La Rioja (Spain)` |
  | both | both | `La Rioja (province, Argentina)` |

- **A qualifier names the parent in full** (`La Rioja (Argentina)`), never by a code.
- **A qualifier in a native field is localised.** It is the country card's own `placeOri`, in the row's
  language: `Limburg (Nederland)`, `Jura (Suisse)` (2026-09-26).
- **A type is never appended to a name that already carries it, and a country never takes a country
  qualifier.**
- **A trailing qualifier such as `Georgia (country)` is part of the answer.**
- What each type's qualifier says is set in `types.tsv`. Mexico City's qualifier is `district` (ruled
  2026-09-20 by Jens Nordmark (@nordmarj)). A qualifier that vanishes leaves a card with no unique answer.
- <a id="abkhazia-ruling"></a>**The Abkhazia ruling** (2026-11-22, by Jens Nordmark (@nordmarj)). A de facto
  state's country card that shares its name with a card of the state claiming it takes a type qualifier, as
  `Georgia (country)` does: `self-declared state` has the qualifier `state` in `types.tsv`, so Abkhazia's card
  answers `Abkhazia (state)` and Georgia's autonomous republic `Abkhazia (Georgia)`. The other self-declared
  states share no name with another card and take none.
- **A capital takes no country qualifier**: see [the capital-qualifier rule](#capital-qualifier-rule)
  (2026-11-22).

---

<a id="languages"></a>
## 6. Languages

<a id="language-field"></a>
### `language`: the language of the native names

`language` names **the languages that `placeOri` and `capitalOri` are written in**, in the same order,
separated by ` / `. It is not the polity's official language. The IPA labels follow it.

<a id="language-name-rule"></a>
#### Language names (the language-name rule)

Ruled 2026-11-22 by Jens Nordmark (@nordmarj). **A language is named by its English Wikipedia article title,
without the word "language" or a disambiguator**, in `language`, `otherLanguages` and the IPA labels alike. Where
the name in use is a redirect, the article it leads to gives the name: `Boro`, not `Bodo` (`Bodo language` leads
to `Boro language (India)`). A language is named once on a card, never under two names. Examples: `Guarani`, not `Guaraní`; `Sotho`, not
`Sesotho`; `Tswana`, not `Setswana`; `Northern Sotho`, not `Pedi` or `Sepedi`; `Northern Sámi`, not `Northern
Sami`; `Mandarin Chinese`, not `Mandarin`; `Chewa`, not `Chichewa` or `Nyanja`; `Sinhala`, not `Sinhalese`;
`Maldivian`, not `Dhivehi`; `Tetun`, not `Tetum`; `Kiribati`, not `Gilbertese`.

<a id="region-language-rule"></a>
#### A region with a language of its own

Ruled 2026-09-26 by Jens Nordmark (@nordmarj). Cells also call this *the region-language rule*, *the
at-most-two rule* and *the own-language-first rule*.

- The native names are written in **the region's official languages other than English, at most two**:
  `Catalunya / Cataluña`.
- **The region's own language comes first**, meaning the one that is official in the region but not
  nationwide. Where neither language is the region's own, the order is the one its statute gives; where the
  statute gives none, the more spoken language comes first.
- **Where English is the national language, the region's own language stands alone**: `Cymru`, `Hawaiʻi`,
  `தமிழ்நாடு`.
- **A name spelled alike in both languages is written once**: `Galicia`, `Barcelona`.
- **A third official language, and any language that is recognised but not official, goes to
  `otherLanguages`.**

<a id="english-rule"></a>
#### English in `language` (the English rule)

Ruled 2026-10-01 by Jens Nordmark (@nordmarj). **`language` always names English where English is an official
or main language of the place.** The native name itself stays non-English.

- English comes **first where it is the main language**: Ireland `English / Irish` with `Éire`; Canada `English
  / French`; the Isle of Man `English / Manx`.
- **Otherwise English comes after the native name's languages**: Kenya `Swahili / English`.
- English is then never listed in `otherLanguages`.
- Gibraltar's `language` is English alone; Spanish and Llanito go in `otherLanguages` (ruled 2026-09-30 by Jens
  Nordmark (@nordmarj)).

<a id="country-card-rule"></a>
#### A country card (the country-card rule)

Ruled 2026-10-01 by Jens Nordmark (@nordmarj). A country card takes **its official languages other than
English, at most two**. They are in the constitution's order, or failing that the more spoken first:
Tajikistan `Тоҷикистон / Таджикистан`, `Tajik / Russian`.

<a id="nationwide-rule"></a>
#### Official across the country, official in each region (the nationwide rule)

Ruled 2026-10-01 by Jens Nordmark (@nordmarj). Cells also call this *the national rule*.

- **A language official across the whole country is official in each of its regions.**
- **Where the region's own law names its official languages, that law decides.** Examples: a Swiss canton's
  constitution, a Belgian language area, a Canadian province, an Ethiopian region.
- **A language official in only part of a region is not official in it.** Italian and Hungarian, official in a
  few Slovenian municipalities, are not official in Slovenia's statistical regions.
- **Sámi in Norway** (ruled 2026-10-02 by Jens Nordmark (@nordmarj)) is shown only on the counties whose
  official name has a Sámi form by law. The country card and the other counties are Norwegian; Sámi goes in
  the country card's `otherLanguages`.

<a id="several-languages-rule"></a>
#### A region with several languages of its own

Ruled 2026-09-30 by Jens Nordmark (@nordmarj).

- **Keep the national language as one of the two, and add the region's most-spoken language of its own.** The
  rest go in `otherLanguages`.
- Where two of the region's own languages spell the name alike, it is written once. `language` then names both,
  in order of speakers, and one IPA half is labelled with each (`Erzya / Moksha : [...]`).

<a id="skip-when-unsure"></a>
#### Skip a second language where unsure

Ruled 2026-10-01 by Jens Nordmark (@nordmarj). A language joins `language` and `placeOri` only where a stored
source shows it is official in the region. Otherwise the national language stands alone (Panama's comarcas).

<a id="working-language-ruling"></a>
#### Working languages (the working-language ruling)

Ruled 2026-10-03 by Jens Nordmark (@nordmarj).

- **A language that the constitution or a law names as a working language counts as official.** Examples:
  French in Mali, Burkina Faso and Niger; Tigrinya, Arabic and English in Eritrea.
- A lingua franca that no law names does not count.
- A region's names stay in French with the type word (`Région de Kayes`). A national language joins only where
  a stored source gives the name in it; in Niger, Hausa then comes first.
- **English joins `language` where the charter names it a working language too** (Burkina Faso, Niger).
- **Mauritius** (ruled 2026-10-04 by Jens Nordmark (@nordmarj)): section 49 of its constitution is read under
  this ruling, so the cards take French and English. Creole comes first in `otherLanguages`.
- **Ethiopia goes by its constitution only.** Amharic is the federal language, and each region takes its own
  constitution's languages. A government announcement is not a law.

<a id="country-language-rulings"></a>
#### Rulings on languages by country

- **An Indian state is bilingual in the order of its own official-language law** (ruled 2026-10-04 by Jens
  Nordmark (@nordmarj)). The principal language comes first. An additional official language follows where a
  stored source gives the name in it (`Telugu / Urdu`); where many are official, the most spoken one follows.
- <a id="indian-english-ruling"></a>**The Indian English ruling** (2026-11-22, by Jens Nordmark (@nordmarj)).
  Where a stored source makes English official in an Indian state or territory by its own law, `language` names
  English after the native names' languages, as [the English rule](#english-rule) has it, and English leaves
  `otherLanguages`. Where no stored source says so, English stays where the card has it. It applies to
  Puducherry `Tamil / English`, Haryana `Hindi / Punjabi / English`, Ladakh `Hindi / English`, Delhi
  `Hindi / Punjabi / English` (the Delhi Official Languages Act 2000) and Goa `Konkani / Marathi / English` (the
  Goa, Daman and Diu Official Language Act, 1987).
- **A constitution's alphabetical list of languages sets no order** (ruled 2026-10-03 by Jens Nordmark
  (@nordmarj)). Bolivia's departments take Spanish and their own language, the more spoken first: Potosí
  `Quechua / Spanish`.
- <a id="albanian-first-ruling"></a>**The Albanian-first ruling** (2026-10-13; extended 2026-10-20 and 2026-10-28,
  by Jens Nordmark (@nordmarj)). In North Macedonia, Albanian goes first in `language` wherever it is the
  largest mother tongue, and a plurality is enough. The municipalities' other languages are read from the 2021
  census's mother-tongue figures, with a 5% bar. The country card is `Macedonian / Albanian`.
- **Kazakhstan's region capitals are written in Kazakh and Russian**, as their `placeOri` is (ruled 2026-10-28
  by Jens Nordmark (@nordmarj)).
- **Jersey follows the Guernsey pattern**: `English / Jèrriais`, with French in `otherLanguages`.

<a id="other-languages"></a>
### `otherLanguages` (the otherLanguages rule)

Ruled 2026-09-26 by Jens Nordmark (@nordmarj); narrowed 2026-10-01.

- **What it holds**: the region's official or major languages **other than the ones `language` names**, in
  English, separated by ` / `.
- **Order**: official languages first, then other major ones; within each group, by number of speakers.
- **Length**: name four, then `and N more`.
- <a id="what-counts"></a>**What counts as a language here.** A stored source must say one of the following
  about that language in that region:
  - **The language is official there.** On a country card, a language counts as official if it is recognised
    nationally and official in one of the subdivisions (Ethiopia's Afar), or if the state recognises it as a
    national language by law or decree (<a id="national-recognition-rule"></a>the national-recognition rule,
    2026-10-01).
  - **At least 5% of the region's people speak it natively.** The figure must be for the region, not the
    country. An ethnicity share does not count, and neither does second-language knowledge.
    - <a id="everyday-language-rule"></a>**The everyday-language rule** (2026-10-01): where the census asks
      only for the everyday or main language, that figure counts (Burgenland Croatian at 6.1%).
    - **A figure from before 2000 does not count** (ruled 2026-10-02 by Jens Nordmark (@nordmarj)).
    - A figure that a dated source cites counts as of that source's date.
  - **The region's official name is in that language.**
- **Exceptions ruled by country** (each by Jens Nordmark (@nordmarj)):
  - **Mali's regions show `-`** (2026-10-13). No regional speaker figures exist for the 13 national languages
    that the 2023 constitution makes official.
  - **Burkina Faso's regions and card show `-`** until the law listing its national languages is stored
    (2026-10-20).
  - Ghana's eleven government-sponsored languages do not count, because no law names them (2026-10-13).

<a id="none-found-convention"></a>
### The none-found convention

Ruled 2026-09-30 by Jens Nordmark (@nordmarj) for `currency`, `callingCode` and `ccTld`, and 2026-10-01 for
`otherLanguages`.

- **The rule:** where the sources a reader checked hold nothing that qualifies, the empty cell gets `-`, the
  deliberate blank. Its `#why` names the pages read.
- **On `currency`, `callingCode` and `ccTld`, `-` means the unit has no value of its own.** No registry says
  that a unit has *no* currency or code, so the registry page read is cited without a quote.
- **On `otherLanguages`, `-` means no language met the bar in the stored sources.** It does not mean the
  region has no other language.
- **A none-found never overwrites a value.** On a filled cell, it becomes a question.

---

<a id="category-and-status"></a>
## 7. Category and status

<a id="specific-term-rule"></a>
### The category is the specific term (the specific-term rule, the 2026-09-18 ruling)

Ruled 2026-09-18 by Jens Nordmark (@nordmarj).

- **`category` is the most specific type English uses for the unit**: its statutory or status-bearing phrase.
  It is not the head noun the unit shares with its siblings. Examples: `economic prefecture`, `self-governing
  city`, `metropolitan province`, `autonomous province`, `centrally-governed city`.
- **A register or Wikidata value that flattens a country's types into one word does not override it.**
- **Counting sources does not settle it.** What decides is whether English uses the specific phrase for this
  unit.
- **A category is a type, never the unit's own name.** Nigeria's `Federal Capital Territory` is a `capital
  territory`.
- <a id="local-form-ruling"></a>**The local-form preference.** A local type word is used where English has
  adopted it for that country: Ukraine's `raion`, Russia's `oblast`, France's overseas `commune` (*the
  French-administration convention*). Otherwise the English word is used.
  - <a id="rayon-ruling"></a>**The rayon ruling** (2026-09-18) is an example: English titles every Azerbaijani
    unit `… District`, so the category is `district`.
- **A specific category is corrected only on evidence that it is wrong** (ruled 2026-09-30 by Jens Nordmark
  (@nordmarj)): the term is not current, not used in English, or names a different tier. Where unsure, a
  question is raised.

<a id="cook-islands-ruling"></a>
### A governing body is not a unit type (the Cook Islands ruling)

Ruled 2026-09-18 by Jens Nordmark (@nordmarj). A term that names a council (`island council`, `town council`)
names the body, not the place it governs. The exception is where English names the unit by its council. The test
is whether English uses the word for the place.

<a id="rule-7"></a>
### A city that is its own first-level unit

Ruled 2026-09-30 by Jens Nordmark (@nordmarj). Such a city takes **the type its law gives it, where English uses
that type**. Otherwise it is a `city`. It never takes its own name as its type.

<a id="status"></a>
### Status

- **`status` is the political status as a phrase**: `sovereign state`, `oblast of Russia`, `unincorporated
  territory of the United States`.
- <a id="sevastopol-ruling"></a>**A status asserts the sovereignty the card is drawn under** (ruled 2026-09-18
  by Jens Nordmark (@nordmarj); *the Sevastopol ruling*). A region drawn under a country names that country:
  `UA-40` is a `city in Ukraine`. The de facto position goes in `note`.
- **A first-level status is composed from its `category`** (ruled 2026-09-18 by Jens Nordmark (@nordmarj)). To
  correct it, correct the category or the country card's `placeEng`. A term that already uses its *of* is
  joined with *in*.
- <a id="polity-type-rule"></a>**A country card's `category` is its polity type** (*the polity-type rule*, also
  *the level-0 category rule*): the status phrase without the sovereign's name (ruled
  2026-09-20 by Jens Nordmark (@nordmarj)). The overlap between the two fields is accepted.
- **A status names the parent state wherever one exists**: `Crown Dependency of the British Crown` (ruled
  2026-09-20 by Jens Nordmark (@nordmarj)).
- Where no sovereign is asserted, `status` repeats the category: Western Sahara is a `disputed territory`;
  Somaliland and Northern Cyprus are `self-declared state`s; Bir Tawil is an `unclaimed territory` (ruled
  2026-09-20 by Jens Nordmark (@nordmarj)).
- <a id="siachen-ruling"></a>**The Siachen ruling** (2026-11-22, by Jens Nordmark (@nordmarj)). The Siachen
  Glacier's card asserts no sovereign: Natural Earth files it under a `Kashmir` that is no card, so its `admin`
  is empty, as Palestine's is, and its `status` repeats its category, `disputed territory`. India's holding
  and Pakistan's claim are in `note`.

---

<a id="capitals"></a>
## 8. Capitals

<a id="capital-conventions"></a>
### What a capital is (the capital conventions)

Ruled 2026-09-15, with the later rulings marked.

- <a id="station-rule"></a>**The station rule.** An uninhabited island or territory has no capital, **unless
  one research station or base is there**, which is then the capital. Examples: Port-aux-Français, Olonkinbyen,
  Willis Island, Sand Island (Midway), King Edward Point, Diego Garcia. Several competing stations make no
  capital (Antarctica).
- **A territory run from outside, with no seat of its own, has none** (Tuamotu-Gambier). Tajikistan's Districts
  under Central Government Jurisdiction, run from Dushanbe, which lies outside them, have none (ruled
  2026-10-01 by Jens Nordmark (@nordmarj)).
- <a id="no-named-seat"></a>**The no-named-seat ruling.** A parish or similar local unit whose sources name no
  seat has none. A dataset's silence is not absence; a source saying that no seat is named is.
- <a id="village-name-rule"></a>**The village-name rule.** A unit named after a village takes that village
  (Anguilla's districts).
- **A municipality administered directly by the state has itself as its capital** (Beijing, not Tongzhou). A
  city-state's capital is itself.
- **Western Sahara: Laayoune. The West Bank: Ramallah.**
- <a id="no-seat-in-law"></a>**No seat in law, no capital** (ruled 2026-10-01 and 2026-10-13 by Jens Nordmark
  (@nordmarj); also called *the Iceland ruling*). Where no settlement holds a seat in law and the unit has no
  office of its own, the capital is `-`. Examples: Iceland's eight regions, Antigua's parishes.
  - A Jersey parish takes its own name, the address of its Parish Hall.
  - A Guernsey parish takes its own name, the locality of its Douzaine's office (ruled 2026-10-18 by Jens
    Nordmark (@nordmarj); this overturned the `-` of 2026-09-15).
- <a id="seat-rule"></a>**The seat rule (the working-seat rule)** (ruled 2026-10-04 by Jens Nordmark
  (@nordmarj)). Where the seat in law and the working seat differ:
  1. the capital is **where the unit's own government actually sits, if that is inside the unit**;
  2. otherwise it is the seat in law;
  3. with no seat in law, it is `-`.

  The other seat goes in `note`. Three city halls make a split capital (`Gwangju / Muan / Suncheon`).
  **Jerusalem Governorate** is the exception: its capital is Jerusalem, the official seat, though its offices
  sit in al-Ram (ruled 2026-11-16 by Jens Nordmark (@nordmarj)).
- **A split capital lists every city**, ` / `-separated, in `capitalEng`, `capitalOri` and `capitalCoord`.

<a id="capital-names"></a>
### The capital's names

- **`capitalEng` is the capital's English Wikipedia article title**, with any disambiguator dropped (ruled
  2026-10-13 by Jens Nordmark (@nordmarj)): `Fort-Dauphin`, `Port Blair`.
- <a id="capital-qualifier-rule"></a>**A capital carries no country qualifier (the capital-qualifier rule)**
  (ruled 2026-11-22 by Jens Nordmark (@nordmarj)). `capitalEng` and `capitalOri` hold the city's name alone:
  `Victoria`, not `Victoria (Canada)`; `La Paz / Chuqiyapu`. Capitals that share a name are told apart on the
  card instead: the `capital -> place` front's caption names every other card of the same level whose capital
  shares the city's name, whatever its country (British Columbia's names Labuan).
- <a id="city-government-form-rule"></a>**`capitalOri` is the official native name that the city's own
  government uses** (ruled 2026-09-30 and narrowed 2026-10-01 by Jens Nordmark (@nordmarj)). Cells also call
  this *the capitalOri rule* and *the official-name rule*.
  - Examples: `مدينة الكويت`, `Ciudad de México`, but `Buenos Aires`. A long ceremonial or statutory form is
    left out.
  - **Where no stored source shows the city government's own form, the question stays open** (ruled 2026-10-13
    by Jens Nordmark (@nordmarj)).
  - **The native-language Wikipedia's form stands where no city-government page is reachable** (ruled
    2026-10-19 by Jens Nordmark (@nordmarj)).
- <a id="dzongkhag-town-ruling"></a>**The dzongkhag-town ruling** (2026-10-19, by Jens Nordmark (@nordmarj)). A
  Bhutanese town that is named after its dzongkhag and run by the dzongkhag administration takes the
  dzongkhag's name without `རྫོང་ཁག`; Dagana takes the official `དར་དཀར་ན་`. This is an exception to the
  city-government-form rule. A town with a government of its own (a thromde) is not covered.
- **Algeria's `capitalOri` takes what the sources give** (2026-09-15). It is Arabic only where no source names
  the town in Tamazight, and a half is never copied from the province's name.

<a id="capital-sources"></a>
### Choosing a capital from the sources

- **No source is an authority.** Natural Earth's points are matched by position, never by name; Wikidata's
  capital by code; Overture's capital division never by name.
- **A capital is filled where at least two of the three sources agree, and no source that answers names another
  city.** A disagreement becomes a question.
- **`-` is written only where a source says there is none.** Silence is missing data.

<a id="r1"></a>
### The capital's coordinate: rule R1

Ruled 2026-10-19 by Jens Nordmark (@nordmarj). `capital_points.tsv` holds R1's choice for each city.
`capitalCoord` in `notes.tsv` holds the decided value, as `lat, lon` to six decimals.

- **Only a point that names the capital counts.** Its name folds to the city's name, or is a near-spelling of
  it, or the point lies within 5 km of a point that does. Two sources agreeing on another place never count.
- **With three sources, a point more than 10 km from both of the others is dropped.**
- **Order of preference**:
  1. GeoNames (populated places only)
  2. Overture
  3. PCGN, the UK's Permanent Committee on Geographical Names, whose factfile centres are the fifth opinion
     (ruled 2026-10-21 by Jens Nordmark (@nordmarj))
  4. English Wikipedia
  5. Natural Earth

  A point inside the card's region is preferred.
- **When a question is raised**:
  - the chosen point is more than 10 km from another counting source;
  - a lone point lies more than 2 km outside its region;
  - every point names another place.
- **A point outside its region that two sources agree on stands.**
- **A question is answered by writing the `capitalCoord` cell.** A row whose `#why` names a person is never
  overwritten.
- A capital point is joined to its source by exact title or id, never by a search. A disambiguation page, or a
  page that is not a settlement, gives no point.

---

<a id="ipa"></a>
## 9. Pronunciation (IPA)

<a id="ipa-convention"></a>
### The IPA convention

Ruled 2026-09-15.

- **`placeIPA` and `capitalIPA` give the pronunciation of the native name the card shows**, in the source's
  delimiters.
- **They come only from a source**, except where the rules below allow a derived or joined IPA.
- **A bilingual name's halves are labelled**: `Sinhalese : [...] - Tamil : [...]`. A one-language IPA takes
  no label.
- <a id="same-name-rule"></a>**The same-name rule.** A region and its capital, or a country, that share the
  identical native name share its pronunciation.
- <a id="longer-form-rule"></a>**The longer-form rule.** Where the source transcribes a longer form, the
  matching words stand. A type word or augment that the card drops is left out of the IPA (Serbian `округ`,
  Kinyarwanda `u Rwanda`).
- **A type word the card keeps need not be pronounced** (Greenland's `Kommunia` cards, Hsinchu's `市`).
- <a id="sole-dialect-form"></a>**The sole-dialect-form convention** (*the dialect rule*). A dialect
  pronunciation of the same name is accepted where it is the only one. A different variety with its own
  standard is not a dialect spelling: the Cantonese or Wu offered for Chinese cards gave way to Mandarin.
- <a id="typo-rule"></a>**The typo rule.** An obvious typo in a source's IPA is fixed and noted. A doubtful
  one is not.
- **A pronunciation that the source generates from a template is the source's IPA**, as rendered.
- **French IPA stands for a Seychellois Creole name**, labelled `French`.
- <a id="another-language-rule"></a>**IPA in another language of the place stands where the card's language
  has none**, labelled with that language (*the another-language rule*). Since 2026-10-18 this needs a stored
  source saying the language is spoken in the region ([the IPA conventions of 2026-10-18](#ipa-2026-10-18)).
- <a id="nDjamena-convention"></a>**The N'Djamena convention** (2026-09-26) is an example: French IPA of the
  same spelling stands for an Arabic-script name, labelled `French`.
- **geosiwk is a source where no other is.** A stored source replaces a geosiwk value. A geosiwk value that is
  itself defective is a question.

<a id="whole-name-rule"></a>
### The whole-name rule

Ruled 2026-09-26 by Jens Nordmark (@nordmarj).

- **An IPA must cover the whole name the card shows.** A partial one is `none-found`.
- <a id="bilingual-rule"></a>**The bilingual rule** (ruled 2026-09-30 by Jens Nordmark (@nordmarj)). Where only
  one half of a bilingual name has a sourced IPA, that half stands, labelled, and the other half is left out.
  The whole-name rule applies within each half.
- **An unsourced IPA is brought to its source's transcription, however small the difference** (ruled
  2026-09-30 by Jens Nordmark (@nordmarj)).
- **An IPA may be composed by cutting the article or suffix that the card's name drops** from the sourced IPA
  of the full form: `Prizreni` gives Prizren (ruled 2026-10-13 by Jens Nordmark (@nordmarj)).
- Mali's IPA tagged `und` (undetermined language) stands where it matches the card's name (2026-09-26).

<a id="haiti-ruling"></a>
### The Haiti ruling

Ruled 2026-09-30 by Jens Nordmark (@nordmarj). On Haitian cards, French IPA stands only where it plainly
transcribes the Creole name the card shows. Where the names differ (`Nòdès` / Nord-Est) and the fit is unsure,
the cell is cleared.

<a id="saba-ruling"></a>
### The Saba ruling

Ruled 2026-10-02 by Jens Nordmark (@nordmarj); bounded 2026-11-17.

- **A native name spelled as in English takes only its native IPA.** Saba's `Saba` and Sint Eustatius's
  `Oranjestad` are Dutch names: their IPA is the Dutch one alone, unlabelled. An English half is dropped, even
  where `language` names English.
- **Where the name only looks English, no English IPA is used** (Saint Martin in Guernsey; Rabat in Malta).
  Where the native name *is* the English word, see [the English-IPA convention of 2026-10-18](#ipa-8).

<a id="joined-ipa"></a>
### Joined and derived IPA

Ruled 2026-10-18 by Jens Nordmark (@nordmarj).

- **Joined word by word.** Where no source pronounces the whole name, its IPA may be joined from the sourced
  IPAs of each word, all in the card's language: Iran's `استان X`, Anguilla's `George Hill`. Every source is
  cited, and `#why` says it is a join.
- **The genitive derivation.** A Latvian `X novads` card, whose name stands in the genitive (`X novada`), may
  take the genitive derived from the nominative's sourced IPA. Since 2026-10-18 this covers Lithuanian too
  ([the IPA conventions of 2026-10-18](#ipa-2026-10-18)).
- <a id="same-spelling-ruling"></a>**The same-spelling ruling** (2026-10-18). A Wiktionary entry for *another
  place* with the identical spelling may supply the IPA, if it is in the card language's own section (Maryland
  for Liberia's county, Boa Vista).
  - **Never a homograph exonym** (ruled 2026-10-19 by Jens Nordmark (@nordmarj)): `Canaries` is not the Canary
    Islands.
  - **Never a line the source itself doubts** (2026-10-19): "Can we verify this pronunciation?".

<a id="ipa-2026-10-18"></a>
### The IPA conventions of 2026-10-18

Ruled 2026-10-18 by Jens Nordmark (@nordmarj). Cells cite them by their content and date ("phonemic over
narrow, an IPA convention of 2026-10-18"); the numbers are the order they were ruled in.

| no. | rule |
|---|---|
| 3 | French Wiktionary's `\mun.du\` is written `[mun.du]` |
| <a id="ipa-4"></a>4 | **Phonemic over narrow**: where an entry gives a phonemic `/…/` and a narrow `[…]`, the phonemic is written |
| 17 | the same applies where the phonemic form is on a separate line |
| 7 | **"Another language of the place"** counts only where a stored source says the language is spoken in the region (not Yoruba for Imo) |
| <a id="ipa-8"></a>8 | **An English IPA stands, labelled `English : […]`, where a native name spelled as in English has no native IPA**, until a native one is sourced. A line "approximating" the native pronunciation (Taranaki) stands unlabelled. *Bounded 2026-11-17: only where the native name is the English word* ([below](#ipa-2026-11-17)) |
| <a id="ipa-9"></a>9 | **The accent rule (the Commonwealth convention)**: where an entry lists several accents, a Commonwealth English card takes Received Pronunciation, or a line naming the card's own country. Liberia takes the US line. Portuguese-speaking Africa takes "(Portugal)". Spanish takes the general line. Arabic and Persian take the standard or formal line. Vietnamese takes Hà Nội. Otherwise the first variant is taken. Chinese tone notation stays as written |
| 11 | an entry defining only a surname, given name or demonym may supply the IPA, if it is in the card's own language section |
| 12 | **a dialect-only line** stands only where the dialect is spoken in the region (Sidon takes the Levantine line) |
| 13 | an obvious typo is fixed and noted (the typo rule) |
| 14 | **a split capital's sourced seat stands alone**, labelled with the city's name (`Dehradun : /…/`) |
| 15 | the genitive derivation covers Latvian and Lithuanian |
| — | **the type word is pronounced only where the name without it is just an adjective** (`Western Province`, Cameroon's `Région du Sud`) |
| — | same-spelling entries count only from the card's own language section |

<a id="english-uk-line"></a>
### English names: the UK line

Ruled 2026-10-20 by Jens Nordmark (@nordmarj). English IPA takes the UK line (Received Pronunciation). The US
line is used only where no UK line exists.

<a id="ipa-2026-11-17"></a>
### The rulings of 2026-11-17

Ruled 2026-11-17 by Jens Nordmark (@nordmarj).

- **A dictionary's single US line counts on any card.** In a word-by-word join it counts word by word, with its
  syllable spaces removed.
- **An English IPA stands where the native name *is* the English word** (Pembroke, Malta: the town's own sense).
  It never stands where the name only looks English ([the Saba ruling](#saba-ruling)). Rotuma takes its
  article's English IPA, which overturns its drop of 2026-10-02.
- A rule-derived IPA ignores a Wiktionary entry that was refused as another word.
- `مدينة` before a city's name is a kept type word, as `市` is: Gaza City is `/ɣaz.za/`.
- **A place named by a common word takes that word's IPA** (Vingt-Cinq).
- **A join that mixes `/…/` with `[…]` is written in brackets.** Slashes are used only where every part is in
  slashes.

<a id="ipa-by-rule"></a>
### IPA derived by rule

Adopted 2026-10-19 by Jens Nordmark (@nordmarj). This overturns "only from a source" for these languages alone.

- **Where no source pronounces the name, the IPA may be derived from the spelling for four languages**, whose
  rules matched the sourced IPA closely when measured against it:
  - Spanish: *distinción* in Spain and Equatorial Guinea, *seseo* elsewhere; Mesoamerican `x` and spellings
    that are not Spanish are refused.
  - Macedonian.
  - Azerbaijani.
  - Tanzanian Swahili. Kenya and Uganda are refused.
- The result is phonemic, in slashes, and leaves out the type word the card keeps. Its `#why` says "derived by
  rule", with the language's measured rate.
- **A sourced IPA always replaces a derived one.**
- **Every other language was measured and refused.** Their spelling does not show stress, length, vowel quality
  or vowels. Epitran is adopted for no language, because it gives no stress.
- <a id="vocalised-ipa"></a>**Arabic-script names from a sourced vocalisation** (ruled 2026-10-20, adopted
  2026-10-21 by Jens Nordmark (@nordmarj)).
  - **The route:** an Arabic-script name may take IPA derived from the BGN/PCGN romanisation printed beside it in
    the PCGN Toponymic Factfiles. The target is Standard Arabic.
  - **Adopted below the usual bar.** The rule matched 88% of the sourced Standard-Arabic names exactly, under
    the usual 93%.
  - **Algeria is left out** (ruled 2026-10-22 by Jens Nordmark (@nordmarj)): only 11 sourced names exist to
    measure against.
  - **The Arabic Wikipedia route is not used.** It derives from a fully vowel-marked form on an Arabic Wikipedia
    page, and matched 81%.
- **Tok Pisin and Somali are source-only** (ruled 2026-10-22 by Jens Nordmark (@nordmarj)). No rule measured
  above 79%.

<a id="wikipron-join"></a>
### The confirmed word-by-word join

Ruled 2026-10-20 by Jens Nordmark (@nordmarj). WikiPron's lists are used only to find each word's headword.

- **A join is written only where every word is confirmed**: the stored Wiktionary entry's first line, for the
  card's language and variety, must give the same transcription.
- **The join is refused** for a language written without its vowels, a heteronym, a homograph that differs
  only in capitalisation, and a short list of refused words.
- A type word the card keeps is left out, unless the rest of the name is only an adjective.

---

<a id="audio"></a>
## 10. Audio

<a id="audio-rule"></a>
### Native recordings (the rule of 2026-11-01)

Ruled 2026-11-01 by Jens Nordmark (@nordmarj). `placeAudio` and `capitalAudio` hold one recording per half of the
native name, chosen in `audio.tsv`.

- **Which recordings count**:
  - A recording counts by IPA's rules: it says the native name the card shows, in that half's language. A type
    word the card keeps need not be spoken.
  - **A recording of the same spelling in the same language counts from every source**, whatever it was
    recorded for.
  - **The exception is a script written without its vowels** (Arabic, Persian, Dari, Urdu, Pashto, Hebrew,
    Sindhi, Kurdish, Kashmiri, in an Arabic or Hebrew script). There, only a recording that Wikidata ties to
    the card's own item counts.
- **Refused**: a recording over 10 s, one with no MP3 transcode, and four recordings in the wrong language.
- **Order of choice**:
  1. tied to the item before spelling-only;
  2. exact before less-type-word;
  3. Lingua Libre, then a file Wikidata names, then one a Wiktionary entry names, then a Commons title;
  4. then the filename.

  **The licence never decides.**
- **A row whose `#why` names a person is never overwritten.** A person's `-` says the half has no recording.
- A recording is used only while its spelling is still that half's, so a renamed card loses a stale recording.
- **Where audio plays**: beside the name wherever a card prints it, **never where the name is the answer**
  (ruled 2026-11-01; fronts included since 2026-11-08, by Jens Nordmark (@nordmarj)).
- <a id="audio-more-sources"></a>**More native sources** (ruled 2026-11-08 by Jens Nordmark (@nordmarj)):
  - Commons series with a variety tag (`Pt-pt …`, `Es-mx-…`), joined on exact spelling and language.
  - The half's own-language Wiktionary, counting only a file whose title tags the half's language.

<a id="english-audio-rule"></a>
### English recordings (the rule of 2026-11-07)

Ruled 2026-11-07 by Jens Nordmark (@nordmarj). `placeEngAudio` and `capitalEngAudio` follow `audio_eng.tsv`. The
rules of the native table apply, with these differences:

- **UK first**: exact before less-type-word; then UK, US, another national variety, unspecified; then tied,
  source, filename.
- **An English type word is dropped as a suffix or a prefix** (`County Clare`, `Canton of Jura`), but never so
  that a bare common word is left (`Eastern`).
- **A spelling that differs only by diacritics counts**, ranked below exact (`Bogota` for Bogotá).
- **A lower-case first letter is no match by rule** (`salt`, `same`): only a person's row can take one.
- **Where the native field already plays the same file, or says the same English name in another recording,
  the English field stays empty.** One button, not two.
- An MP3 original counts as its own transcode.
- The Voice of America Pronunciation Guide's public-domain MP3s are a source, each credited to its entry page.

---

<a id="registry-fields"></a>
## 11. Registry fields: ISO codes, currency, calling code, ccTLD

Ruled 2026-09-29 by Jens Nordmark (@nordmarj). One principle covers all of them: **the value an international
registry assigns to the unit itself.**

<a id="iso-fields"></a>
### The four ISO code fields

- **They are generated from the card's code, never typed.** A country card shows ISO 3166-1 alpha-2, alpha-3
  and numeric (three digits, leading zeros kept).
- A subdivision shows its own ISO 3166-2 code. Failing that, it shows the agreed key; a one-source key is
  shown only where a reading confirmed it. Failing that, it shows nothing.
- **A provisional code is not shown.** [Ruled codes](#codes) win.

<a id="currency"></a>
### `currency`

- **The name and code that ISO 4217 List One lists for the entity**, with the name in English: `Swedish krona
  (SEK)` (ruled 2026-09-30 by Jens Nordmark (@nordmarj)).
- **A dependency's own issue counts where List One gives it a code**: `Falkland Islands pound (FKP)`.
- **Where List One gives the local issue no code of its own, the entity's listed currency is written.** The
  Jersey, Guernsey and Manx pounds are written `Pound sterling (GBP)`.
- <a id="several-currencies"></a>**The several-currencies ruling** (2026-09-30, by Jens Nordmark (@nordmarj)).
  Where List One lists several currencies, every one is written, ` / `-separated, in List One's order: Haiti
  `Haitian gourde (HTG) / US dollar (USD)`.
- **List One's fund codes (IsFund) are left out** (ruled 2026-10-05 by Jens Nordmark (@nordmarj)), such as
  Bolivia's Mvdol and Switzerland's WIR.

<a id="calling-code"></a>
### `callingCode`

- **The ITU's country code, or a range that the ITU's plan gives the unit as its own** (`+599 7`). Never an
  ordinary area code.
- **A North American Numbering Plan member takes `+1` and the area code(s) that identify it**: `+1 264`; `+1
  809 / +1 829 / +1 849` (ruled 2026-09-30 by Jens Nordmark (@nordmarj)). The United States and Canada take
  `+1`.

<a id="cctld"></a>
### `ccTld`

- **Only a domain that IANA classes as country-code**, ASCII only. Internationalised ccTLDs are skipped, so
  Russia is `.ru`, not `.ru / .рф`.
- A domain never delegated is not written (`.xk`, `.bl`, `.eh`).

<a id="registry-none"></a>
### No value of its own

- **`-` is written wherever no registry assigns the unit a value of its own** (ruled 2026-10-05 by Jens
  Nordmark (@nordmarj); see [the none-found convention](#none-found-convention)). This covers most
  subdivisions. Examples: no List One entry (Kosovo, Northern Cyprus, Somaliland); no ITU code or range (Åland,
  the Crown Dependencies, Saint Helena).
- A value the unit only shares with its country belongs on the country card.

---

<a id="captions-notes"></a>
## 12. Captions, notes and `mem`

<a id="captions"></a>
### Captions (the caption ruling)

Ruled 2026-09-26 by Jens Nordmark (@nordmarj).

- **A caption is written only where it tells the learner something**:
  - the region has several flags or emblems (a state flag against a civil flag, a seal against arms);
  - the design is shared with another place;
  - the flag is a plain field that could be anyone's.

  A single ordinary flag gets no caption.
- **Sentence case** (`State flag`, `Civil ensign`, `Coat of arms`). Never the region's name, never a blazon or
  a description. *The 2026-09-26 normalisation* rewrote existing captions to this form. It applied the
  convention and was not a ruling on any single picture.
- **The level is automatic** (ruled 2026-09-26 by Jens Nordmark (@nordmarj)). Every caption opens with `Flag of
  a country or territory`, `Flag of a subdivision` or `Flag of an area within a country` (or `Emblem of …`),
  composed when the deck is built. A flag or emblem question must have one answer.
- <a id="shared-flag-ruling"></a>**The shared-flag ruling** (2026-09-17, by Jens Nordmark (@nordmarj)). A
  genuinely shared flag names its other holders: `Also flown by Dubai`. This makes the answer unique.
- <a id="near-duplicates"></a>**Near-duplicates are named only at their own level** (ruled 2026-09-26 by Jens
  Nordmark (@nordmarj)).
  - Identical pictures: `Same design as the flag of X`.
  - Pictures differing only in shade or ratio: `Near-identical to X's flag`.
  - A small emblem difference: nothing.
  - Every look-alike at the same level is named, in any country, with no cap on the count. This replaced *the
    groups-of-two-or-three rule*. Pairs a learner can tell apart are not named.
  - **Look-alikes are found on the rendered pixels, never the filename.**

<a id="note-field"></a>
### The `note` field (the note ruling)

Ruled 2026-09-26 by Jens Nordmark (@nordmarj); widened 2026-09-30.

- **Most cards have no note.** A note is short, cited context that the answer needs:
  - a contested or unrecognised status, a sovereignty dispute included (2026-09-30): *"Sovereignty disputed by
    Spain; on the UN list of non-self-governing territories"*;
  - a split or seasonal capital;
  - a recent rename or merger;
  - a shared flag.
- A first-person remark imported from geosiwk is rewritten as cited context where it carries a needed fact, and
  dropped otherwise.

<a id="mem"></a>
### `mem`

`mem` is the learner's own field (ruled 2026-09-26 by Jens Nordmark (@nordmarj)). The project leaves it empty.
geosiwk's maintainer notes there were dropped (2026-11-15).

---

<a id="flags-and-emblems"></a>
## 13. Flags and emblems

<a id="whose-picture"></a>
### Whose picture it is

- <a id="council-flag-ruling"></a>**A council's flag is the unit's flag** (ruled 2026-09-17 by Jens Nordmark
  (@nordmarj)). This holds where the council is the unit's own statutory government,
  governing that ground and no other. It never covers a ministry's, agency's or party's mark, nor a separatist
  body's.
  - **So is a council's logo, as the unit's emblem**, where a device survives the masking of the name (ruled
    2026-10-13 by Jens Nordmark (@nordmarj)).
- <a id="national-flag-refusal"></a>**The national-flag refusal** (ruled 2026-09-17 by Jens Nordmark
  (@nordmarj)). A unit's flag or emblem is never its country's own national flag or emblem: a dependency or
  subdivision that flies its sovereign's flag has no flag of its own on its card. The country is found by code,
  and a picture is refused where it is the same picture as the national one.
- <a id="appointed-administration"></a>**The appointed-administration ruling** (2026-10-13, by Jens Nordmark
  (@nordmarj)). An appointed administration's mark counts where it is the unit's only government.
- **A Turkish governorship's logo is the province's emblem**, and **an Omani governorate's own logo is the
  governorate's** (ruled 2026-09-27 and 2026-10-01 by Jens Nordmark (@nordmarj)).
- <a id="tabuk-ruling"></a>**The Tabuk ruling** (2026-10-02, generalised 2026-10-18, by Jens Nordmark
  (@nordmarj)). A capital city's municipal mark is not the province's: a Saudi regional amanah's logo is not
  the province's emblem.
- <a id="not-every-banner"></a>**Not every banner is a government's.** None of these is the region's:
  - a ministry's logo, a development agency's brand mark, a political party's banner, a fan design;
  - a team's banner (2026-10-21);
  - an intercommunal authority covering the same ground;
  - a separatist body's flag. A file credited to a separatist body's site is rejected (ruled 2026-10-18 by Jens
    Nordmark (@nordmarj)).
- <a id="addu-ruling"></a>**The Addu ruling** (2026-10-19, by Jens Nordmark (@nordmarj)). A brand mark on a plain
  field is no unit's flag. Auckland's and Waikato's council logos on white are examples (2026-10-28).
- <a id="generic-logo-ruling"></a>**The generic-logo ruling** (2026-10-13, by Jens Nordmark (@nordmarj)). A
  generic logo that every unit of a country shares, differing only in the name (the national arms with the
  province's name), is not the unit's emblem. A town's own arms are not covered by this ruling.
- <a id="coextensive-capital-ruling"></a>**The coextensive-capital ruling** (2026-09-17, by Jens Nordmark
  (@nordmarj)). A region coextensive with its capital takes the capital's insignia: Banaadir is Mogadishu, and
  Mariehamn's sub-region is the town. Whether the two coincide is settled from the ground.
  - León's and Rivas's departments in Nicaragua take their capital city's arms as shared (ruled 2026-10-19 by
    Jens Nordmark (@nordmarj)), since the departments have no government of their own.
- <a id="capital-rule"></a>**Otherwise the capital's file is not the region's** (*the capital rule*). A file
  the region's own Wikidata item names counts as the region's. So does a filename carrying the unit type or
  the region's name. Sofia Province is still refused its city's flag.
- <a id="portugal-ruling"></a><a id="official-flags-kept"></a>**If official, it is kept** (ruled 2026-11-17 by
  Jens Nordmark (@nordmarj): "if official we keep them"; also cited as *the Portugal ruling*).
  - **An officially adopted flag or emblem is kept**, even where it is a curiosity flown nowhere.
  - **A flag that no source shows officially adopted, and that is flown nowhere, is not the unit's.** This
    covers a hypothetical design and a compiler's drawing. An example is a Jersey parish banner that FOTW calls
    "not confirmed by photographic evidence" (2026-10-19).
  - This narrows the Portugal ruling of 2026-10-03, which had skipped district flags as "curiosities flown
    nowhere" whether official or not. The owner may revisit whether official curiosities are excluded.
  - **Portugal's districts still have no flag or emblem on their own ground.** They have had no government since
    2011, so [the no-government rule](#no-government-no-flag) gives tier B. The capital city's insignia is not
    a district's either (2026-10-03).
- **Both the seal and the coat of arms belong on a card** (2026-09-14; restated 2026-10-07 by Jens Nordmark
  (@nordmarj)). A state with both takes both, one per emblem slot. This is the verdict `both`. An official seal
  is included as the second emblem even where arms exist.
- **A picture in the wrong role is moved, not discarded** (2026-09-17).
- <a id="historical-range"></a>**A bracketed year range marks a picture as historical only when the range
  closes on a year** (2026-09-17; refined 2026-10-19 by Jens Nordmark (@nordmarj)). `(2025-)` and
  `(1971–present)` are current.
- **A pre-merger Vietnamese logo is not the merged province's** unless the new unit adopted it. Continued
  official use counts as adoption (2026-10-03).
- **Where geosiwk has a current picture, it is the better card.** A Commons file replaces it only on evidence
  that it is outdated. <a id="premasked"></a>**A copy that arrived with its legend blanked is replaced by its
  unredacted original**, masked to the name only (ruled 2026-10-06 by Jens Nordmark (@nordmarj); 365 replaced
  as one block on 2026-11-15).
- A choice put forward from a page ranks below a person's choice.

<a id="layers"></a>
### Sources and layers

- **Layer order** (ruled 2026-09-27 by Jens Nordmark (@nordmarj)). A later source gives a role only where every
  earlier one has nothing for it:
  1. checked files;
  2. Wikimedia Commons;
  3. government sites;
  4. this project's own compositions and redraws;
  5. Flags of the World.
- **No social media**, archived copies included.
- **A government file is used exactly as published.** Each row names the page and a stored snapshot.
- <a id="compose"></a>**Composed flags are allowed, with a recipe** (ruled 2026-09-27 and 2026-10-13 by Jens
  Nordmark (@nordmarj)).
  - Allowed: tribands, diagonal fields and bands, a turned emblem, and a published logo recoloured to one
    colour (recolouring counts as composing).
  - Every colour, ratio or turn that FOTW does not state is recorded as a choice.
- <a id="crop-rule"></a>**The crop rule** (ruled 2026-10-13 by Jens Nordmark (@nordmarj)). An emblem seen only in
  a lockup or a photograph is cropped from the publication where it is at least about 150 px; otherwise it is
  left empty.
- <a id="fotw-images"></a>**Flags of the World**:
  - **Its drawings are a source** (ruled 2026-10-19; terms used as written since 2026-10-21, by Jens Nordmark
    (@nordmarj)). Each row names the contributor and "FOTW Flags Of The World website at
    https://www.fotw.info/".
  - **The 5% limit in FOTW's terms counts the images the deck ships**, not the images stored.
  - **A FOTW drawing ranks above an unfree file.**
  - **An emblem cut out of a FOTW flag drawing counts from 140 px.** It is presentation, not alteration.
  - **A contributor's own drawing counts**, even where it was drawn from a photo posted on social media. A
    picture that another source only "located", "sent" or "posted" is not a contributor's.
  - **A picture FOTW calls "not confirmed by photographic evidence" is not used.**
- **A free file named by a non-English infobox is fine. A non-free upload on any wiki except English Wikipedia
  is refused** (ruled 2026-10-18 by Jens Nordmark (@nordmarj)).
- A dead government site's page or file may be used as the Wayback Machine archived it. A site whose
  robots.txt refused us is refused in the archive too.
- **A file matched only by a Commons category is matched by category membership, never by its name.**

<a id="licence-ruling"></a>
### Licences (the licence ruling)

Ruled 2026-10-20 by Jens Nordmark (@nordmarj). **Every picture the deck ships must be shareable, at least
non-commercially.**

- **What is replaced.** `Fair use` files, `No licence stated` files (and compositions from them) and
  `PD-Amtliches Werk` files are replaced. A blank licence is looked up.
- <a id="fair-use-ruling"></a>**This supersedes the fair-use ruling** (2026-09-17; confirmed superseded
  2026-11-17 by Jens Nordmark (@nordmarj), as is the `PD-Amtliches Werk` acceptance below). That ruling kept English
  Wikipedia's non-free uploads as `Fair use`, to be pulled if the deck were shared.
- **It also supersedes the acceptance of `PD-Amtliches Werk` files** (2026-10-19). That acceptance rested on
  German law, not the source country's.
- **The routes to a shareable picture, in order**:
  1. a free file found elsewhere;
  2. public domain under the source country's own law on official symbols, with the statute stored and cited;
  3. our own redraw: a flag from its design, arms from their blazon, a logo or seal redrawn.

  A picture with no route is dropped, but only after the whole set has been measured.
- **Rulings of 2026-10-21 (Jens Nordmark (@nordmarj))**:
  - **A Commons licence tag is taken as stated**, unless a deletion request is open.
  - **A FOTW drawing is a free file**, and so is an emblem cut out of one.
  - **Laws that free a country's official symbols**:
    - Nigeria's Copyright Act 2022 s. 3(c) frees the States' seals and arms and the FCT emblem, but not the
      Rivers State logo.
    - Moldova's Law 230/2022 art. 8(c) frees the districts' arms and flags.
    - Cameroon's Law 2000/011 s. 3(4)(c) frees the regional emblems.
  - **A free re-interpretation of a pictorial device is no free file.** There is no "too simple to copyright"
    route.
  - The Turkish governorships' shared terms forbid copying, so their logos are redrawn where FOTW has no
    drawing.
- <a id="redraws"></a>**Our own redraws** (ruled 2026-10-23 by Jens Nordmark (@nordmarj)):
  - **Licence and credit**: hand-written SVG under CC0, credited "drawn by World in Flashcards Map Renderer".
    Each carries a recipe: the design source and its cite, the colours, the simplifications and the lettering.
  - **A simplification is fine; a changed shape is not.**
  - **Names are left out.** Mottos are kept, as paths.
  - **Initials drawn as the device are design.**
  - **A scene with figures, or a photo collage, gets one draft.** It is dropped if the draft is poor. **An
    emblem built from photographs** takes another free source first (FOTW, then a web search), else a stylised
    redraw, which is dropped if it does not look good (ruled 2026-10-21 by Jens Nordmark (@nordmarj)).
- **Every non-FOTW picture's credit names its licence and our changes** (ruled 2026-11-15 by Jens Nordmark
  (@nordmarj)): `[CC BY-SA 4.0; changes: keyline added, name masked on the question]`.
- **A picture's credit reaches the note's `source`.**

<a id="no-image"></a>
### A region with no flag or emblem of its own

`no_image.tsv` records a ruling that a region has none. **A file found later always outranks it.**

- <a id="no-government-no-flag"></a>**A unit with no government of its own flies no flag of its own** (ruled
  2026-09-27 by Jens Nordmark (@nordmarj); *the flag ruling of 2026-09-27*). Each row names its tier:

  | tier | evidence |
  |---|---|
  | **A** | a source says there is none: *"The province has no official flag"* |
  | **B** | a law or official definition shows that no body exists which could adopt one |
  | **C** | only secondary sources show that |

- **A silent source is not evidence.** A sentence about flags says nothing about emblems. The sentence must be
  about this unit and still true.
- **A region's own FOTW page outweighs a seed ruling** (ruled 2026-09-27 by Jens Nordmark (@nordmarj)).
- <a id="provisional"></a>**Provisional**:
  - **An emblem searched for in every source and not found is provisionally empty** (ruled 2026-10-06 by Jens
    Nordmark (@nordmarj); *the 2026-10-06 rule*, *the provisional-emblem rule*). This applies where the unit's
    body could bear one. Its `#note` starts `provisional`, and it carries no tier.
  - **So is a flag searched for in every source and not found** (ruled 2026-10-18 by Jens Nordmark
    (@nordmarj)).
  - **So is a flag or emblem known to exist with no usable file.** Its reason says where it was seen (Facebook
    only, a dead site, an unsourced drawing, no licence, below the crop floor).
- <a id="emblem-is-flags-charge"></a>**An emblem that exists only as the central charge of the flag the card
  draws is left empty, decided** (ruled 2026-10-18 by Jens Nordmark (@nordmarj)). It is not repeated.
- **A role whose only picture a ruling dropped is decided**, and its reason names the ruling. A flag whose only
  device is a dropped logo goes with it.
- **Country blocks** (each by Jens Nordmark (@nordmarj)). The tier is written per row; these set the pattern:
  - **China's 30 mainland province-level units: no flag and no emblem, tier B** (2026-10-03). They come under
    the national ban of 18 November 1997 on local flags and emblems. Hong Kong and Macao are exempt; Tibet is
    included (2026-10-07).
  - **Slovenia's 12 statistical regions: none** (2026-10-01).
  - **The units new in 2024-2026 have none** (2026-10-21). Angola's four new provinces and Guinea's Siguiri and
    Beyla are tier B; Congo's three new departments are provisional.
  - **The Maldives' atoll councils were abolished on 27 May 2026**, so the units are tier B (2026-10-21). An
    abolished council's mark is no longer current.
  - **Palestine's 16 governorates fly no flag, tier A, and their emblems are provisional** (2026-11-17). Every
    office mark seen was the State's eagle.
  - **Algeria's 11 wilayas of 2026 are re-checked after their assemblies are elected on 28 November 2026.**

---

<a id="masking"></a>
## 14. Masking lettering that names the answer

On the question side, lettering that names the region is painted out. The picture as published is kept in the
`…Full` field for the answer side.

<a id="masking-rules-2026-09-16"></a>
### The masking rules of 2026-09-16/17

- <a id="geographic-outline-ruling"></a>**A geographic outline is not lettering** (2026-09-16; *the
  geographic-outline ruling*). A region drawn on its own flag stays.
- <a id="cannot-be-masked"></a>**A picture that cannot be masked cleanly is kept, not dropped** (2026-09-16).
  This is subject to the legibility rule below.
- <a id="hand-boxes"></a>**Lettering the detector missed or misread is boxed by hand** (2026-09-16). A hand row
  in `masks.tsv` keeps the literal phrase "by hand". Its `kind` is one of these:
  - `hand`;
  - `exact`, painted exactly as drawn, with the ground estimated round the letters;
  - `clone DX DY`, filled from the picture that far away;
  - `keep`, restoring a thin line unpainted.

  A box may be four corners, an `arc` or a `band`. `clone` and `keep` were added 2026-11-15 by Jens Nordmark
  (@nordmarj).
- **The paint is the ground the lettering sits on**, not the field around it (2026-09-17).

<a id="latin-legibility-ruling"></a>
### Lettering a learner can read (the Latin-legibility ruling, the script ruling)

Ruled 2026-09-17 by Jens Nordmark (@nordmarj); refined 2026-09-26.

- **Mask wherever a distinctive device survives.**
- **Where a name in Latin letters is the only thing that tells a picture apart from its country's other
  subdivisions', the picture stays unmasked.** A pure wordmark is included. A card that teaches by reading is
  poorer, but still a card.
- **Where the only distinction is lettering in another script, the picture is dropped.**
- The cases that set it: Nigeria's `GOVERNMENT OF X STATE` seals with the national arms inside are kept
  unmasked; India's state emblems, which differ only by the state's name in Devanagari and other scripts, were
  dropped.
- <a id="japanese-emblem-ruling"></a>**A character stylised into an official symbol is a design, not text**
  (2026-09-26; *the Japanese-emblem ruling*). Kyoto's 京 and Nara's ナ are examples.
  <a id="keelung-ruling"></a>**The Keelung ruling** (2026-09-29) applies this to Keelung's stylised `基隆`.
  Lienchiang's logo, masked to the `馬祖` wordmark in Chinese script, is dropped.

<a id="name-only-rule"></a>
### Masks cover the name, and nothing else (the name-only rule)

Ruled 2026-09-26 by Jens Nordmark (@nordmarj).

- **Only the region's name is painted.** A line box holding more is narrowed to the name.
- **Left unpainted**: mottos, dates, the country's name and type words.
- **A capital's name is painted only where the region is its capital.**
- <a id="type-phrase-never-masked"></a>**A type phrase is never masked** (ruled 2026-11-15 by Jens Nordmark
  (@nordmarj)): `ESTADO DE`, `GOBIERNO REGIONAL DE`, `REGIONE`. This holds even where `placeOri` carries the
  phrase, because every sibling's picture shares it.

<a id="what-counts-as-the-name"></a>
### What counts as the name

Ruled 2026-09-26 by Jens Nordmark (@nordmarj).

- **Initials are the name**: `РС`, `SP`, `N★C`. <a id="gpaa-cers-rule"></a>**A unit's seal may write its
  initials together with its type words** (`GPAA`, `CERS`, `PTRC`). From four letters, these count as the name
  too (ruled 2026-10-06 by Jens Nordmark (@nordmarj); *the GPAA/CERS rule*).
- **Another form of the name is masked only if a learner could read the answer off it**: `MNI SOTA` is masked;
  `CALPE` is not. `M.T.S.N.` is not the name (2026-09-29).
- **Lettering in a script that no recogniser reads is still lettering.** It is masked by hand and checked by
  eye.

<a id="masking-limits"></a>
### Limits on the name-only rule

- <a id="low-resolution-scroll"></a>**A low-resolution scroll may lose its whole line** (ruled 2026-09-28 by
  Jens Nordmark (@nordmarj)). This covers sources of 180-300 px, where name-only paint scars the scroll.
- <a id="ring-band-ruling"></a>**A ring's lettering band may be painted whole** (ruled 2026-09-29 by Jens
  Nordmark (@nordmarj)), where letter-by-letter paint damages it.
- <a id="name-drawn-as-design"></a>**A name drawn as the design is kept** (ruled 2026-09-29 by Jens Nordmark
  (@nordmarj)). Examples: Ica's vine stems, the Cordillera's `CAR`, Mersin's wordmark. Further cases:
  - Ilinden's large И (2026-10-06).
  - Gyeonggi's ㄱㄱㄷ (2026-10-07).
  - Republika Srpska's gold `РС` monogram and Santiago Metropolitan Region's `GRM` (2026-11-15).
- **A scar over the name is accepted where no paint does better**: Kericho's flag and Demir Hisar's emblem
  (2026-10-06), Struga's emblem (2026-10-18).
- **A patch over the name itself is not damage**, however visible. A card that answers itself is worse than a
  scar.
- Card examples, each ruled by Jens Nordmark (@nordmarj):
  - Pasco's emblem masks `CERRO DE PASCO` whole (2026-11-16).
  - South Dakota's seal keeps `THE MOUNT RUSHMORE STATE` (2026-11-16).
  - Crete's flag is kept unmasked (2026-10-19).

---

<a id="maps"></a>
## 15. Maps

Maps are drawn by the World in Flashcards Map Renderer. These rules decide what a map shows. The tables named
here list the cards each rule applies to. **Where a rule applies only to listed cards, the list decides, never
a threshold worked out when the map is drawn.**

<a id="map-style"></a>
### The set

- **Every map follows one specification**: an equal-area projection centred on the region; the region
  highlighted with its siblings around it; a locator; and a detail panel where the region is small.
- **A subdivision card always shows its own region on the main map.**
- **A card must not give the answer away.** It shows context, never a label.

<a id="which-dataset"></a>
### Which dataset draws each card

- **Each country's subdivisions come from one named dataset.** The choices are GADM, geoBoundaries, Overture
  Maps, OCHA COD-AB and the World Bank. A dataset is chosen per country on evidence of which draws the current
  units.
- <a id="closest-area-rule"></a>**A country card's own outline is Natural Earth's** unless Natural Earth fails
  it (ruled 2026-09-26 by Jens Nordmark (@nordmarj)). Then it moves to the passing source closest to the
  published area (*the closest-area rule*).
  - A replacement may not be coarser than 4 card pixels between vertices.
  - It must draw every island that two other sources draw at 5 px or more (2026-09-27).
- **Where a country's card and its subdivisions come from different datasets, their edges are left as drawn**
  (ruled 2026-11-16, accepted 2026-11-17 by Jens Nordmark (@nordmarj)).
- **Reorganised units** (each ruled by Jens Nordmark (@nordmarj)):
  - Angola: the 21 provinces, from the World Bank (2026-10-20).
  - The Republic of the Congo: from Overture, with its three new departments cut out by COD-AB's districts as
    the laws list them (2026-10-20).
  - Guinea: Siguiri and Beyla carved out of GADM's prefectures (2026-10-20). Beyla takes Kouankan (2026-10-21).
  - Oman: from Overture (2026-10-20).
  - Kiribati: from Overture (2026-11-11).
  - Malta: from Overture, Mġarr gaining its card (2026-11-16).
  - Palestine: its 16 governorates (2026-11-16).
  - Egypt keeps GADM's governorates, Luxor taking the 2009 law's ground (2026-10-20).
  - Uganda keeps its four regions (2026-11-16).
- <a id="other-area"></a>**A governed unit off the tier a country is drawn at takes a third level, "an area
  within a country"** (ruled 2026-11-16 by Jens Nordmark (@nordmarj)). The West Bank and Gaza are examples. It
  is drawn without the subdivisions inside it, as a country card is.
- <a id="repairs"></a>**Repairs are keyed on the defect, never on a region.** Each is guarded by a point from an
  independent source, and stops applying once the dataset is fixed. Examples:
  - Malta's MT-61/MT-63 swapped in geoBoundaries.
  - Spanish Wells' cays misfiled in GADM.
  - Perim moved to Ta'izz (2026-11-11).
  - Greenland's municipalities gain the islands geoBoundaries leaves out (2026-11-11).
  - The Halaib Triangle given to Red Sea Governorate (2026-11-12).
  - Ground Overture's new release left in no region given back (2026-10-27).
  - Isla de Perejil, held by nobody, cut out of Ceuta (2026-11-12).
- **An atoll country's units are their region's whole area cut to the land** (the Maldives, the Marshall
  Islands; 2026-10-19).

<a id="level-0"></a>
### Which country cards exist

- **The set**: Natural Earth's countries, plus map units with an ISO code that the countries layer lacks, plus
  a few units with no ISO code.
- **The eight official Antarctic claims** (`XQ…`) and **the unclaimed land** (`XQN`, Marie Byrd Land) are cards,
  framed on Antarctica and drawn on no other card (2026-10-03, 2026-10-04).
- **Four de facto states are country cards**: Transnistria, Abkhazia, South Ossetia, and the Sahrawi Arab
  Democratic Republic (ruled 2026-10-07 by Jens Nordmark (@nordmarj)).
  - A claim counts as far as it exists on the ground, so the SADR is Western Sahara east of the wall.
  - They are drawn over the recognised state's unchanged cards.
- **`XAD` is both Sovereign Base Areas** (2026-09-14). **Baikonur and Guantánamo are drawn inside their lessors
  and as cards of their own** (ruled 2026-09-21 by Jens Nordmark (@nordmarj)).
- **Rockall (`XRK`) is an islet** whose sea is a 12-nm territorial sea, with no EEZ (2026-10-04). Its land, Peter
  I Island's and Serranilla Bank's come from Overture, because Natural Earth misplaces them (2026-10-11,
  2026-10-17).
- **Remote islands get a card wherever a dataset draws them** (2026-09-14).

<a id="water"></a>
### Water

- <a id="lake-rule"></a>**The lake rule (the world lake rule)** (ruled 2026-09-27 by Jens Nordmark (@nordmarj)).
  A lake that Natural Earth omits is added from Overture where all of these hold:
  - it covers at least 16 px on some card;
  - it is not salt, intermittent or seasonal water;
  - it is not river water;
  - it is not mostly sea.

  `added_lakes.tsv` lists each lake, chosen by geometry and kind, never by name. A named exception is
  excluded; Monaco's Baie de Saint-Laurent, Caño Tiburones and Timor-Leste's braided river bed are examples
  (2026-10-22, 2026-10-27, 2026-11-12). Rows added by a person cite their own ruling.
- **An added lake is drawn by the ordinary lake rule on every card whose canvas shows it.**
- **An added lake replaces Natural Earth's copy of it** (ruled 2026-11-08 by Jens Nordmark (@nordmarj)). This
  applies to a Natural Earth lake that lies at least 25% inside the added one.
- **A border that crosses a lake is drawn like any other border** (2026-11-08).
- **Overture's water replaces Natural Earth's lakes in the detail panel of the cards `panel_water.tsv` lists**
  (ruled 2026-09-26 by Jens Nordmark (@nordmarj)).
- <a id="subdivision-water"></a>**A subdivision loses the water its country's card tints**, only for the cards
  `subdivision_water.tsv` lists (ruled 2026-09-27 by Jens Nordmark (@nordmarj)). A row below the default size
  floor names its own ruling.
- **Sea zones**:
  - **Internal waters are tinted like the zone** (2026-09-25).
  - **A zone hole within 12 nm of the land is internal water**, for the countries `zone_hole_countries.tsv`
    lists (2026-11-12).
  - **An unclaimed EEZ zone goes to the card whose land lies at least 90% within 30 km of it.** It is never
    assigned by the sovereign field.
  - **An island's zone is tinted on its sovereign's card only where the island is on the main map or in the
    detail panel** (2026-10-13, 2026-10-16).
- **A contested or joint sea is tinted on the card that holds it**, as on land (ruled 2026-11-12 by Jens
  Nordmark (@nordmarj)). `disputed_seas.tsv` names each zone's holder. Where no one state holds it (the South
  China Sea), no card tints it.
- **Rivers are drawn only in the deeper detail panels** (2026-10-17).

<a id="ice-shelves"></a>
### Antarctica's ice shelves

Ruled 2026-10-13 by Jens Nordmark (@nordmarj). **Natural Earth's Antarctic ice shelves are land, drawn lighter.**

- A frame centred near a pole is drawn on the pole, the same way up every time.
- A box spanning every longitude holds every longitude.
- **Framing counts grounded land only**: a remote island's distance is measured to land without the shelves.
- `ice_shelf_cards.tsv` lists the cards that draw them.

<a id="framing"></a>
### Framing

- **A card framed on its own region frames every significant part within 2,000 km** (approved 2026-10-06 by
  Jens Nordmark (@nordmarj)). A part the frame would cut is brought in, or left out entirely, but never cut
  (2026-10-13).
- <a id="overseas"></a>**A subdivision counts as overseas only beyond 400 km of its parent's mainland** (ruled
  2026-10-11 by Jens Nordmark (@nordmarj)).
  - Prince Edward Island, Ceuta and Kaliningrad are framed on their country.
  - The Canaries, the Azores and Hawaii are framed on themselves.
- **A claiming unit is framed on its state's outline together with the ground it claims** (Sevastopol,
  Famagusta).
- <a id="held-ground"></a>**A card is framed on the ground it holds** (ruled 2026-10-22 by Jens Nordmark
  (@nordmarj)). A part of its land at least half inside an area another state holds is left out of its framing,
  but is still drawn and hatched. The exception is a card where such parts are half its land or more.
- `outlying_cards.tsv` lists the cards these rules change.

<a id="panels"></a>
### Locator, detail panel and frames

- **Every card is offered both panels** (ruled 2026-10-16 by Jens Nordmark (@nordmarj)).
  - **The locator is drawn on every card**, in a neutral style.
  - **The detail panel is offered to a region under 10,000 px² of visible land.** It is not drawn where it would
    not show the region closer than the map does.
  - **Empty detail panels are kept.**
- **The locator's dot marks the region's largest landmass**, or each landmass where there are at most three,
  and never a centroid that falls outside the region (2026-10-13).
- **A region whose land would take more than 5% of the 12,000 km locator gets a wider one**, up to 18,000 km.
- <a id="panel-frames"></a>**Each view outlines the next closer one** (ruled 2026-11-01 by Jens Nordmark
  (@nordmarj)).
  - The frame is a 1.5 px dark-red rectangle.
  - No frame is drawn under 8 px; a large frame is drawn however large.
  - A detail frame drawn replaces the main map's ring.
  - A detail frame reaching past the main map is cut at its edge, for the cards `frame_clip_cards.tsv` lists
    (2026-11-12).
- **Natural Earth's Pacific Groupings are not drawn** (2026-11-01).
- <a id="panel-rings"></a>**The detail panel's rings** (ruled 2026-10-17 by Jens Nordmark (@nordmarj); the
  cards `panel_ring_cards.tsv` lists):
  - **An island in the card's own lake is ringed only where its ring fits in the lake's open water.**
  - **A small part is ringed only where it lies further than one ring-width from a readable part.**
  - **Rings that would overlap are merged into one.**
- <a id="panel-zoom"></a>**A near-empty detail panel takes a floor of its own** (ruled 2026-10-16, 2026-10-20 by
  Jens Nordmark (@nordmarj); the cards `panel_zoom.tsv` lists).
  - **The floor.** A region under 45 px takes the widest floor of 30/20/15/10/7.5/5/3/2 km that gives it about
    60 px while keeping the whole region.
  - **Overture's coast.** An `overture` row draws the panel from Overture's land, divided among the units by
    their polygons. Land within 400 m of a unit is that unit's; land further from every unit is nobody's.
  - **Reefs.** A reef is the card's only where it touches the card's land and no other land. Land that is
    nobody's but lies on the card's own atoll is the card's; an atoll is found by geometry alone.
  - **Island panels.** An island panel whose dataset coast is coarse or stale takes Overture's coast (ruled
    2026-11-07 by Jens Nordmark (@nordmarj)).
- **A bank, under 1 km² of land with no other land within 25 km, draws its reef in the detail panel**
  (2026-10-13).

---

<a id="disputed"></a>
## 16. Disputed areas and claims

Ruled 2026-10-08 by Jens Nordmark (@nordmarj), with later rulings marked. `disputed_areas.tsv` records who holds
and who claims each area. `disputed_units.tsv` lists the subdivisions that claim one. `disputed_cards.tsv` lists
the cards that draw a dispute.

<a id="fill-follows-ground"></a>
- **Fill follows the ground, and border lines follow international recognition.**
  - Whoever holds an area fills it, an annexation few recognise included (Crimea by Russia, the Golan by
    Israel).
  - Every card draws the most recognised border lines on top: the World Bank's official boundaries, and the
    line on the ground dashed where no border is recognised (the Line of Control, the Moroccan wall).
- **How an area is drawn**:
  - The holder's card draws it in its fill, hatched.
  - The other claimants' cards draw it as claimed ground.
  - Where no claimant holds it, it is drawn neutral.
  - Where no one claims it (Bir Tawil), nothing is drawn.
  - An area too small to show meaningfully (under 150 px²) is left out, or drawn as an outline alone.
- **Who holds each area is researched case by case.** Natural Earth's note is a lead, never copied.
- **A card claimed in its entirety by another state is never hatched on its own card.** Taiwan, Kosovo and the
  de facto states are examples; the claim goes in `note`. The claimants' cards still show it.
- **A subdivision card is hatched only where it is itself the claimant's unit for the area** (Quneitra for the
  Golan).
- **A claimant's subdivision gets a card under the claimant**, its `note` explaining the status: Russia's six
  under GOST codes, Taiwan Province `CN-TW`, Guayana Esequiba `VE-GE`.
  - The four oblasts annexed in 2022 are Russia's claim, not under its control: Ukraine's ground, drawn as
    claimed on Russia's cards (ruled 2026-10-22 by Jens Nordmark (@nordmarj)).
  - Their cards show the symbols the Russian-installed administrations adopted.
- **Palestine holds Gaza and the West Bank**, with no stated claimant. Jerusalem and Bethlehem governorates are
  the claiming units for East Jerusalem, and for Gilo and Har Homa (ruled 2026-11-16 by Jens Nordmark
  (@nordmarj)). **Palestine has no sovereign** (2026-11-15). Its occupation goes in `note`. **Nor has the Siachen Glacier a sovereign** ([the Siachen ruling](#siachen-ruling), 2026-11-22).
- **Settled or not drawn**: South Korea's constitutional claim to the whole peninsula is not drawn. Swains Island
  is settled. Israel as a whole, the Latrun and Jerusalem no man's lands, Mount Scopus and Rockall are drawn as
  no one's claim. The UNDOF zone is Syria's claimed-not-held ground.
- <a id="line-weight"></a>**A border line is weighted by where it runs** (ruled 2026-11-16 by Jens Nordmark
  (@nordmarj)).
  - A piece within 2.5 px of the card's own country (its region, its siblings, the parent gap and the ground it
    claims) keeps the strong style.
  - Elsewhere it is drawn light: 0.9 px, grey, dashed.

---

<a id="defunct"></a>
## 17. Defunct units

Ruled 2026-10-03 by Jens Nordmark (@nordmarj).

- **A unit that a boundary dataset still draws, but that no longer exists as drawn, moves to `defunct.tsv`.**
  Its whole row is kept, with the reason, its successors and the source.
- **A defunct unit's map is rendered, but it is not part of the deck.** Its media is stored apart, so a later
  unit given the same code cannot collide with it.
- **Not defunct: a tier that is no longer administrative but is still the reference first-level division, with
  no successor at its level.** It stays, with a `note`. Libya's 22 shabiyat and Iceland's regions are examples.
- <a id="northern-cyprus-rule"></a>**The Northern Cyprus rule** (2026-10-03, by Jens Nordmark (@nordmarj)). A
  unit that a later reorganisation redrew keeps its card, drawn as before. It carries a `note`, and its stale
  flag stays open until a dataset draws the new units. Botswana's seven districts are an example (2026-10-28).
- **Kept with a `note`** (ruled 2026-10-28 by Jens Nordmark (@nordmarj)):
  - Saint-Barthélemy's two traditional parishes.
  - Haa Dhaalu.
  - A card whose unit governs a second atoll (Kili with Bikini).

---

<a id="tags"></a>
## 18. Continents and tags

<a id="m49"></a>
### Continents: UN M49

Ruled 2026-10-29 by Jens Nordmark (@nordmarj).

- **The continent and region come from UN M49**, never from Natural Earth's continent field. Tags are nested:
  `WiF::continent::africa::western-africa`.
  - The region is M49's intermediate region where it has one, else its sub-region.
  - The Americas are split into North and South.
- **A card M49 does not list takes a row in `m49_placements.tsv`.** It is placed by the M49 country its ground
  lies in. It is placed by its M49 region instead wherever naming a country would read as a claim (Kosovo,
  Taiwan, the de facto states).
- **A subdivision takes its country's placement, unless it lies on another continent.** Those subdivisions are
  listed in `m49_subdivisions.tsv`: Asian Russia (`asia::northern-asia`, a project region), Hawaii, Indonesia's
  Papua provinces, Sinai, Turkey's European provinces, the Canaries, Ceuta, Melilla and San Andrés.
- **No second regional scheme is added** (the Sahel, the Levant).

<a id="tag-rules"></a>
### Tags

Each ruled by Jens Nordmark (@nordmarj). Tags are derived from the code, never hand-written per card.

- **Level** (2026-11-09; third level 2026-11-16): `WiF::level::country-or-territory`, `WiF::level::subdivision`,
  `WiF::level::other-area`.
- **Country** (2026-11-08): the country card's name, nested, with accents folded:
  `WiF::country::Sao_Tome_and_Principe`. A dependency's subdivisions carry the sovereign's tag too. Palestine
  has none, having no sovereign (2026-11-15), nor has the Siachen Glacier (2026-11-22).
- **Type** (2026-10-29): every card carries one, a country card its polity type:
  `WiF::region-type::sovereign-state`.
- **Nested only**, with no flat tags beside them (2026-11-09).
- <a id="feature-tags"></a>**Feature tags** (2026-11-09), each held by code:
  - `eponymous`: the capital is named like the place, ignoring a qualifier, the category's words, `of`, `the`
    and `city`.
  - `sovereign-base-area`.
  - `unrecognised-claim`.
  - `limited-recognition`.
  - `claimed-by-another-state`.
  - `enclave`: the three countries Wikidata calls enclaves.
  - `split-capital`.
  - `landlocked` and `doubly-landlocked`: from English Wikipedia's list, recorded in `landlocked.tsv`.
  - `un-non-self-governing`: the current entries of the UN list, from English Wikipedia's list, recorded in
    `un_nsgt.tsv`.
- **Not taken**:
  - a local-tier tag (2026-11-16): the lowest local tier is region-sized in some countries and parish-sized in
    others;
  - an `occupied` tag;
  - geosiwk's `BannerOfArms` and `Identical` tags.

---

<a id="sources"></a>
## 19. Sources and citing

- **Every value cites its sources in `#why`.**
  - A Wikipedia or Wiktionary page is cited at the revision read: `wiki:en:<pageid>@<revid>`.
  - Any other web page is cited as a stored snapshot: `web:<sha16>`.
  - A quote is copied exactly from the source.
- **Pages are found from ids, never from a name search.** A page's code is never read off its title.
- **robots.txt is obeyed.** A published API used under its own etiquette is allowed where robots.txt bars
  crawling (ruled 2026-11-01 by Jens Nordmark (@nordmarj)).
- **Flags of the World, Heraldry of the World and Vexilla Mundi are one compiler's evidence, not an authority.**
  - They are joined to a region on codes, never on a page's name.
  - About its own unit, a compiler's page is stronger than a sentence about a whole country. It does not
    outrank a law, a government's own page or the region's own article.
- **GeoNames is evidence and coordinates, never an authority** (2026-10-18). It is joined on its own Wikidata
  and Wikipedia link rows. A city's point comes only from a populated-place feature.
- <a id="quote-trim"></a>**A quote from a page that is not openly licensed, or from FOTW, is cut to its first 25
  words and "…"** (ruled 2026-11-15 by Jens Nordmark (@nordmarj)). The full text stays in the stored snapshot.
- <a id="geosiwk"></a>**geosiwk** is the Anki deck *Geography Stuff I Wanna Know*.
  - **It is credited where it is the original source** (2026-11-15, 2026-11-16): the note schema, the first
    import, and its own text.
  - **A geosiwk value that another source confirms cites that source.** A value resting on geosiwk alone keeps
    citing it.
  - **The geosiwk import never overwrote a cell, and never matched loosely.** It never imported a country card's
    `placeEng` and never dropped a type word from ours.

---

<a id="index-of-rule-names"></a>
## 20. Index of rule names

The names that cells use for a rule, and where the rule is.

| name in a cell | rule |
|---|---|
| the 2026-09-26 normalisation | [Captions](#captions) |
| the 2026-10-06 rule, the provisional-emblem rule | [Provisional](#provisional) |
| the 2026-10-18 convention, the English-IPA convention | [The IPA conventions of 2026-10-18](#ipa-8) |
| the (-ය) stand-alone convention | [Sri Lanka](#stand-alone-convention) |
| the Abkhazia ruling | [Qualifiers](#abkhazia-ruling) |
| the accent rule, the accent convention, the Commonwealth convention | [The IPA conventions of 2026-10-18](#ipa-9) |
| the additional-official-language ruling, the Mauritius ruling, the Sámi ruling | [Rulings on languages by country](#country-language-rulings) |
| the Addu ruling | [The Addu ruling](#addu-ruling) |
| the Albanian-first ruling | [The Albanian-first ruling](#albanian-first-ruling) |
| another-language rule, another-language-of-the-place convention | [Another language of the place](#another-language-rule) |
| the another-language ruling, the other-language rule | [Another language of the place](#another-language-rule) |
| the appointed-administration rule | [Appointed administration](#appointed-administration) |
| the appointed-administration ruling | [Appointed administration](#appointed-administration) |
| the at-most-two rule, the own-language-first rule, the region-language rule, the two-official-languages rule | [A region with a language of its own](#region-language-rule) |
| the Baikonur ruling | [The Baikonur ruling](#baikonur-ruling) |
| the bare-adjective convention, the adjective-only convention (overturned) | [Bare adjective](#bare-adjective) |
| the bilingual rule, the bilingual one-half rule | [The bilingual rule](#bilingual-rule) |
| the capital conventions, What a capital is | [What a capital is](#capital-conventions) |
| the capital rule (pictures) | [The capital rule](#capital-rule) |
| the capital's-insignia rule, the coextensive-capital ruling, the coextensive-capital rule | [The coextensive-capital ruling](#coextensive-capital-ruling) |
| the capital-qualifier rule | [The capital's names](#capital-qualifier-rule) |
| capitalisation ruling (2026-10-01) | [The title rule](#title-rule) |
| the capitalOri rule, the city-government-form rule, the official-name rule | [The capital's names](#city-government-form-rule) |
| the capitalOri ruling | [The capital's names](#city-government-form-rule) |
| the CERS rule | [What counts as the name](#gpaa-cers-rule) |
| the claimant-symbols ruling | [16. Disputed areas and claims](#disputed) |
| the closest-area rule | [Which dataset draws each card](#closest-area-rule) |
| the coextensive(-capital) rule, the department-uses-capital-arms ruling | [The coextensive-capital ruling](#coextensive-capital-ruling) |
| the Cook Islands ruling | [A governing body is not a unit type](#cook-islands-ruling) |
| the council's-flag ruling, the council rule | [A council's flag](#council-flag-ruling) |
| the council-flag ruling, the council's-page rule | [A council's flag](#council-flag-ruling) |
| the country-card rule | [A country card](#country-card-rule) |
| the country-card ruling, the country-card rule | [A country card](#country-card-rule) |
| the country-name rule, the country-card naming rule | [The country-name rule](#country-name-rule) |
| the crop rule | [The crop rule](#crop-rule) |
| the crop ruling | [The crop rule](#crop-rule) |
| the Crown Dependencies' English convention, the Guernsey pattern | [Rulings on languages by country](#country-language-rulings) |
| the currency ruling | [`currency`](#currency) |
| the dialect rule, the sole-dialect-form convention | [Sole dialect form](#sole-dialect-form) |
| the dialect-line convention | [Sole dialect form](#sole-dialect-form) |
| the dzongkhag-town ruling | [The dzongkhag-town ruling](#dzongkhag-town-ruling) |
| the emblem rule (2026-10-06) | [Provisional](#provisional) |
| the English recordings rule | [English recordings](#english-audio-rule) |
| the English rule | [English in `language`](#english-rule) |
| the English Wikipedia title rule, the title rule | [The title rule](#title-rule) |
| the everyday-language rule | [Everyday language](#everyday-language-rule) |
| the everyday-language ruling | [Everyday language](#everyday-language-rule) |
| the fair-use ruling | [The licence ruling](#fair-use-ruling) |
| the flag ruling (of 2026-09-27) | [No government, no flag](#no-government-no-flag) |
| the French-administration convention, the local-form ruling | [The local-form preference](#local-form-ruling) |
| the generic-logo ruling | [The generic-logo ruling](#generic-logo-ruling) |
| the geographic-outline ruling | [Geographic outline](#geographic-outline-ruling) |
| the geometry rule | [The geometry rule](#geometry-rule) |
| the Ghana convention, the Ghana ruling | [The Ghana convention](#ghana-convention) |
| the GPAA/CERS rule | [What counts as the name](#gpaa-cers-rule) |
| the groups-of-two-or-three rule (overturned) | [Near-duplicates](#near-duplicates) |
| the Haiti ruling | [The Haiti ruling](#haiti-ruling) |
| the hand-box rule | [Lettering the detector missed or misread is boxed by hand](#hand-boxes) |
| the historical-range rule, the historical-picture ruling | [Historical range](#historical-range) |
| the Iceland ruling, the 'no seat in law' rule | [No seat in law](#no-seat-in-law) |
| the Indian English ruling | [Rulings on languages by country](#indian-english-ruling) |
| the IPA convention | [The IPA convention](#ipa-convention) |
| the IPA ruling (2026-09-30) | [The bilingual rule](#bilingual-rule) |
| the Japanese-emblem ruling | [Stylised characters](#japanese-emblem-ruling) |
| the joined-IPA rule, the joined-word-by-word rule | [Joined and derived IPA](#joined-ipa) |
| the Keelung ruling | [The Keelung ruling](#keelung-ruling) |
| the lake rule, the world lake rule, the ordinary lake rule | [The lake rule](#lake-rule) |
| the language rule (2026-10-01) | [`language`: the language of the native names](#language-field) |
| the language-name rule | [Language names](#language-name-rule) |
| the Latin ruling, the Latin-legibility rule, the other-script rule | [The Latin-legibility ruling](#latin-legibility-ruling) |
| the Latin-legibility ruling, the legibility ruling, the Latin-lettering rule, the script ruling, the masking rule of 2026-09-17/26 | [The Latin-legibility ruling](#latin-legibility-ruling) |
| the level-0 rule, the level-0 polity-type rule, the polity-type ruling | [A country card's category](#polity-type-rule) |
| the licence ruling | [The licence ruling](#licence-ruling) |
| the longer-form rule | [The longer-form rule](#longer-form-rule) |
| the low-resolution (scroll) ruling | [Low-resolution scroll](#low-resolution-scroll) |
| the low-resolution ruling, the low-resolution scroll ruling, the low-resolution-scroll ruling | [Low-resolution scroll](#low-resolution-scroll) |
| the Macedonian rule | [IPA derived by rule](#ipa-by-rule) |
| the Montenegro ruling | [The Montenegro ruling](#montenegro-ruling) |
| the N'Djamena convention | [The N'Djamena convention](#nDjamena-convention) |
| the name drawn as the design is kept ruling | [A name drawn as the design](#name-drawn-as-design) |
| the name-covering rule | [The name-only rule](#name-only-rule) |
| the name-only rule, masks cover the name | [The name-only rule](#name-only-rule) |
| the naming policy | [The naming policy](#naming-policy) |
| the national rule, the nationwide rule | [The nationwide rule](#nationwide-rule) |
| the national-flag refusal | [The national-flag refusal](#national-flag-refusal) |
| the national-flag refusal ruling | [The national-flag refusal](#national-flag-refusal) |
| the national-recognition rule | [National recognition](#national-recognition-rule) |
| the native-name ruling, the native-type-word rule, the type-word ruling | [The type-word rule](#type-word-rule) |
| the near-duplicate ruling, the near-duplicate caption rule | [Near-duplicates](#near-duplicates) |
| the no-fair-use rule | [The licence ruling](#fair-use-ruling) |
| the no-flag ruling, the no-body ruling, the tier-B ruling, the country-wide China ruling | [No government, no flag](#no-government-no-flag) |
| the no-image ruling | [A region with no flag or emblem of its own](#no-image) |
| the no-named-seat ruling, the no-seat ruling | [No named seat](#no-named-seat) |
| the no-value-of-its-own rule | [No value of its own](#registry-none) |
| the none-found convention | [The none-found convention](#none-found-convention) |
| the none-found ruling | [The none-found convention](#none-found-convention) |
| the Northern Cyprus rule | [The Northern Cyprus rule](#northern-cyprus-rule) |
| the not-every-banner(-is-a-government's) rule | [Not every banner](#not-every-banner) |
| the not-every-banner-is-a-government's rule | [Not every banner](#not-every-banner) |
| the note ruling | [The note field](#note-field) |
| the otherLanguages rule | [`otherLanguages`](#other-languages) |
| the placeOri conventions, the short-form (placeOri) convention | [The placeOri conventions](#placeori-conventions) |
| the polity-type rule, the level-0 category rule | [A country card's category](#polity-type-rule) |
| the Portugal ruling, the official-flags rule, if official we keep them | [If official, it is kept](#portugal-ruling) |
| the pre-masked copies rule, the pre-masked-copies rule, the pre-masked-copy ruling, the unredacted-original rule | [Pre-masked copies](#premasked) |
| R1, the decided capital point | [Rule R1](#r1) |
| the rayon ruling | [The rayon ruling](#rayon-ruling) |
| the registry-fields ruling | [11. Registry fields: ISO codes, currency, calling code, ccTLD](#registry-fields) |
| the ring ruling | [Ring band](#ring-band-ruling) |
| the ring-band ruling | [Ring band](#ring-band-ruling) |
| the rule for a city that is its own first-level unit | [A city that is its own first-level unit](#rule-7) |
| the rule of 2026-11-01 (audio) | [Native recordings](#audio-rule) |
| the rule of 2026-11-07 (audio) | [English recordings](#english-audio-rule) |
| the Saba ruling | [The Saba ruling](#saba-ruling) |
| the same-name rule | [The same-name rule](#same-name-rule) |
| the same-spelling rule, the same-spelling IPA rule | [The same-spelling ruling](#same-spelling-ruling) |
| the same-spelling ruling | [The same-spelling ruling](#same-spelling-ruling) |
| the seat rule, the working-seat rule | [The seat rule](#seat-rule) |
| the Sevastopol rule | [Status](#sevastopol-ruling) |
| the Sevastopol ruling | [Status](#sevastopol-ruling) |
| the several-currencies ruling | [Several currencies](#several-currencies) |
| the several-languages rule | [Several languages of its own](#several-languages-rule) |
| the shared-flag ruling, the shared-flag rule | [The shared-flag ruling](#shared-flag-ruling) |
| the shared-name convention, the shared-name ruling | [The same-name rule](#same-name-rule) |
| the short-form convention | [The short-form convention](#short-form) |
| the Siachen ruling | [Status](#siachen-ruling) |
| the skip-when-unsure ruling | [Skip when unsure](#skip-when-unsure) |
| the specific-term rule, the 2026-09-18 ruling, the category rule | [The specific-term rule](#specific-term-rule) |
| the station rule | [The station rule](#station-rule) |
| the Tabuk ruling | [The Tabuk ruling](#tabuk-ruling) |
| the Tamazight ruling | [The Tamazight ruling](#tamazight-ruling) |
| the type-word rule | [The type-word rule](#type-word-rule) |
| the typo rule, the obvious-typo convention | [The typo rule](#typo-rule) |
| the UK-line ruling | [English names: the UK line](#english-uk-line) |
| the village-name rule | [The village-name rule](#village-name-rule) |
| the Western Sahara ruling | [The Western Sahara ruling](#western-sahara-ruling) |
| the whole-name rule | [The whole-name rule](#whole-name-rule) |
| the working-language rule | [Working languages](#working-language-ruling) |
| the working-language ruling | [Working languages](#working-language-ruling) |

<a id="index-by-date"></a>
## 21. Index by date

A cell may name a rule by its date alone ("the ruling of 2026-10-13"). Where one day holds several rulings, the
field the cell is about tells you which one applies.

| date | rulings |
|---|---|
| 2026-09-07 | [naming policy](#naming-policy), [qualifiers](#qualifiers) |
| 2026-09-14 | [country-name rule](#country-name-rule); [Montenegro ruling](#montenegro-ruling); provisional codes; seal and arms both; remote islands; `XAD` |
| 2026-09-15 | [IPA convention](#ipa-convention) and its parts; [what a capital is](#capital-conventions); [placeOri conventions](#placeori-conventions); [local-language names](#local-language-name); geosiwk as a source; [adjudicated values](#adjudicated) |
| 2026-09-16 | [masking rules](#masking-rules-2026-09-16): geographic outline, unmaskable pictures kept, hand boxes |
| 2026-09-17 | [Latin legibility](#latin-legibility-ruling); [council's flag](#council-flag-ruling); [coextensive capital](#coextensive-capital-ruling); [shared flag](#shared-flag-ruling); [fair use](#fair-use-ruling) (superseded); [historical range](#historical-range); a picture in the wrong role is moved |
| 2026-09-18 | [specific term](#specific-term-rule), [local form](#local-form-ruling), [rayon](#rayon-ruling); [Cook Islands](#cook-islands-ruling); [status](#status), [Sevastopol](#sevastopol-ruling); `category` editable |
| 2026-09-20 | [Western Sahara](#western-sahara-ruling); [country card's category and status](#status); Mexico City's qualifier; [seeds](#seed) |
| 2026-09-21 | [Baikonur](#baikonur-ruling); Baikonur and Guantánamo drawn |
| 2026-09-26 | [region's own language](#region-language-rule); [`otherLanguages`](#other-languages); [whole name](#whole-name-rule); [N'Djamena](#nDjamena-convention); [captions and near-duplicates](#captions); [note](#note-field); [`mem`](#mem); [name-only](#name-only-rule); [what counts as the name](#what-counts-as-the-name); [script refinement](#latin-legibility-ruling); [Japanese emblems](#japanese-emblem-ruling); [Sri Lanka, New Zealand, Marshall Islands](#single-name-rulings); localised qualifiers; [panel water](#water); [level-0 outlines](#closest-area-rule) |
| 2026-09-27 | [no government, no flag](#no-government-no-flag); [layer order](#layers); [lake rule](#lake-rule); [subdivision water](#subdivision-water); FOTW page over seed; Turkish governorship logos; level-0 replacement criteria |
| 2026-09-28 | [low-resolution scroll](#low-resolution-scroll) |
| 2026-09-29 | [ring band](#ring-band-ruling); [name drawn as design](#name-drawn-as-design); [Keelung](#keelung-ruling); [registry fields](#registry-fields) |
| 2026-09-30 | [title rule](#title-rule); [none-found (registry fields)](#none-found-convention); [several currencies](#several-currencies); NANP; [several languages](#several-languages-rule); [bilingual](#bilingual-rule); [Haiti](#haiti-ruling); [capitalOri](#city-government-form-rule); [a city as its own unit](#rule-7); notes on sovereignty; Gibraltar; ruled codes; closing a question |
| 2026-10-01 | [type word](#type-word-rule); [English rule](#english-rule); [country card](#country-card-rule); [nationwide](#nationwide-rule); [skip when unsure](#skip-when-unsure); [otherLanguages narrowed](#what-counts), [everyday language](#everyday-language-rule), [national recognition](#national-recognition-rule); [none-found (otherLanguages)](#none-found-convention); [no seat in law (Iceland)](#no-seat-in-law); capitalOri narrowed; title capitalisation; Omani logos |
| 2026-10-02 | [Saba](#saba-ruling); [Sámi](#nationwide-rule); pre-2000 census; [Tabuk](#tabuk-ruling) |
| 2026-10-03 | [working language](#working-language-ruling); [Tamazight](#tamazight-ruling); [Gambia](#title-rule); [Portugal](#portugal-ruling); Bolivia's order; [defunct units, Northern Cyprus](#defunct); China's ban; Antarctic claims |
| 2026-10-04 | [seat rule](#seat-rule); Mauritius; India; `XQN`, Rockall |
| 2026-10-05 | fund codes and no own value ([registry](#registry-none)) |
| 2026-10-06 | [provisional emblems](#provisional); [pre-masked copies](#premasked); [GPAA/CERS](#gpaa-cers-rule); scars accepted; [2,000 km framing](#framing) |
| 2026-10-07 | de facto states; seal and arms restated; Tibet |
| 2026-10-08 | [disputed areas](#disputed) |
| 2026-10-11 | [overseas at 400 km](#overseas) |
| 2026-10-13 | [ice shelves](#ice-shelves); [no seat in law](#no-seat-in-law); [capitalEng](#capital-names); [city-government form](#city-government-form-rule); [council logo](#council-flag-ruling), [appointed administration](#appointed-administration), [generic logo](#generic-logo-ruling); [compose and crop](#compose); [IPA by cutting](#whole-name-rule); [Albanian first](#albanian-first-ruling); Mali `-`; framing of cut parts, locator dots |
| 2026-10-16 | [both panels](#panels); [panel floors](#panel-zoom) |
| 2026-10-17 | [panel rings](#panel-rings); Overture panels; rivers; Serranilla |
| 2026-10-18 | [joined IPA, same spelling](#joined-ipa); [IPA conventions](#ipa-2026-10-18); [provisional flags](#provisional); [emblem as flag's charge](#emblem-is-flags-charge); non-English infobox files; Guernsey parishes; amanah logos; GeoNames |
| 2026-10-19 | [R1](#r1); [IPA by rule](#ipa-by-rule); [dzongkhag town](#dzongkhag-town-ruling); [Addu](#addu-ruling); [FOTW images](#fotw-images); León and Rivas; homograph exonyms; open year ranges; atoll countries; English always shown |
| 2026-10-20 | [entity keys and GUIDs](#entity-keys); [licence](#licence-ruling); [English UK line](#english-uk-line); [vocalised IPA](#vocalised-ipa); [WikiPron join](#wikipron-join); reorganised units; Burkina Faso `-` |
| 2026-10-21 | [ISO codes for cards](#codes); licence details; FOTW terms; PCGN; Beyla; new units' no-image |
| 2026-10-22 | [geometry rule](#geometry-rule); [held ground](#held-ground); the 2022 oblasts; Algeria, Tok Pisin, Somali |
| 2026-10-23 | [our own redraws](#redraws) |
| 2026-10-28 | North Macedonia; Kazakhstan's capitals; Botswana; Saint-Barthélemy; Haa Dhaalu |
| 2026-10-29 | [M49 and tags](#tags); capital dot colour |
| 2026-11-01 | [native audio](#audio-rule); [panel frames](#panel-frames); APIs and robots.txt |
| 2026-11-07 | [English audio](#english-audio-rule); island panels take Overture's coast |
| 2026-11-08 | [added lake replaces NE's copy](#water); more audio sources; country tag |
| 2026-11-09 | [tags](#tag-rules) |
| 2026-11-11 | Kiribati; Perim; Greenland's islands |
| 2026-11-12 | contested seas; zone holes; Halaib; Perejil |
| 2026-11-15 | [type phrase never masked](#type-phrase-never-masked); [quote trimming](#quote-trim); licence credits; Palestine's sovereignty |
| 2026-11-16 | [third level](#other-area); Malta; Palestine's governorates; Uganda; [line weight](#line-weight); Jerusalem capital; no local-tier tag |
| 2026-11-17 | [`#why` content-only](#dash-sentinel); [if official, it is kept](#portugal-ruling); [Ghana convention confirmed](#ghana-convention); [IPA rulings](#ipa-2026-11-17); Palestine's governorates' flags and emblems; edges between datasets accepted |
| 2026-11-22 | [capital qualifier](#capital-qualifier-rule); [Indian English](#indian-english-ruling); [language names](#language-name-rule); [Siachen](#siachen-ruling); [Abkhazia](#abkhazia-ruling) |
