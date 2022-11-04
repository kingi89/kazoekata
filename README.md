# kazoekata

**The counter database source for [kazoekata.jp](https://kazoekata.jp/), the Japanese counter lookup tool.**

## Format

The main database file is a CSV (comma-separated values) file in UTF-8. It contains first the heading row, and then the data rows. Column separator is comma (,), and strings are encapsulated in quotes (") as needed.

### Columns

* `used_for`: **Main usage for the counter.** Mandatory. The term should be in singular form, lower-case, and in American English. Example: *bird*.
* `value`: **Explicit count.** Optional. If empty, it is assumed that the counter can be used for any number of items of this type, and that the reading is for the general case. If given, the reading in this row is only valid for the value. Example: *1*.`
* `counter`: **Counter character(s)**. Mandatory. The counter ("suffix") itself in Japanese. Example: *回り*.
* `reading`: **Reading**. Mandatory. For explicit/irregular counts (i.e. `value` not empty), the full reading for both the numerical value and counter together. For general reading (i.e. `value` empty), this must be identical to the `read_key`. Example: *hitori*.
* `read_key`: **General reading specifier**. Mandatory. This value must be identical for all readings (=rows) of the same counter (for different counts). See below for syntax. Example: *-tsubu*.
* `usage_notes`: **Special usage notes**. Optional. A free-text explanation of usage, its limitations, constraints etc, in English. Should be brief, and only used when absolutely necessary. Example: *”tsubo” as an unit of land size*.
* `priority`: **Counter rarity**. Mandatory. A number representing how common/uncommon the counter is (for this particular use). *1* is a very common, everyday counter; *2* a less used or alternative, but still well-known counter; *3* is a rare counter of very limited real-life use; and *4* is an outdated or very rare counter. Example: *1*.
* `source`: **URL source of the information**. Optional. Should be the main source of the information of the row, whether in relation of the existence of the counter, its reading, its rarity or other information. Leave empty if unsure. Example: *https://kazoekata.jp/*.
* `add_date`: **Date of addition of this specific row**. Mandatory. The date must be in `YYYY-MM-DD` format and reflect the day it was added to the database (whether in the original kazoekata repository, or in a fork). Example: *2022-11-04*.
* `revision_date`: **Date of last revision of this specific row.** Optional. The date must be updated each time the row is modified, but left empty when it is initially added to the database. Example: *2023-01-01*.
* `also_used_for`: **Additional usages for the counter.** Optional. Comma-separated list of other uses of the counter (with this reading). The list may also contain irregular plural forms (e.g. *children*). The value must be encapsulated in quotes. Example: *"planet,meteorite,cloud,typhoon,tornado,hurricane"*.

### Reading syntax ###

The reading is written in Latin characters using the modified Hepburn romanization.

For regular (not specific to certain value) reading, both the `reading` and `read_key` values correspond to the reading of the counter itself only. For example, *mai* for *枚*. The reading is prefixed with one or more of the following to signal the "logic" of the general reading:

* `*`: The reading for the counter undergoes a *rendaku* transformation. For example, `*hon` -> *ippon*, *nihon*, etc.
* `-`: For one or two items, the traditional Japanese wago numbers (hito-, futa-) are used instead of the Sino-Japanese kango numbers (ichi, ni).
* `_`: For any number of items, the wago numbers are used.

`-` and `_` are mutually exclusive, but can still coexist with the `*` prefix. The prefixes can be in any relative order.

For `reading` column of an irregular reading, the value is a full reading of both the count and the counter. For example *futsuka* for *2日*. `read_key` for the irregular readings works the same way as it does for regular readings.

### Irregular counters ###

Any *read_key* + *counter* pair with an empty *value* column is considered to be usable for any number of items. To override the default reading for a particular amount of items, an another row with identical *read_key* and *counter*, but with a *value* and value-specific *reading* specified is added.

For counters that only accept certain numbers of items, there should not be a valueless row at all, and the values for which the counter can be used should be specified one-by-one, each having its own row.