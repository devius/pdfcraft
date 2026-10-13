# Georgian interface

> **Last reviewed:** 2026-10-13 · **Last updated:** 2026-10-13 · **Change:** minor (Georgian catalog brought up to date with main, 2,578 entries; previously minor: Georgian interface catalog, dates and font fallback) · **Target:** PdfCraft 0.7.1

Choose **Menu › Edit › Preferences… › Interface language › ქართული**, or leave it on **Auto** and
follow a Georgian system language (`ka`, `ka-GE`, `ka_GE.UTF-8` in `LC_ALL`, `LC_MESSAGES` or `LANG`,
or the preferred-languages list on macOS). The catalog code is `ka`; the control channel's `ui.set`
with `{"key":"language","value":"ka"}` selects it explicitly, as does `--language ka` for the
headless `shot` example. The choice is saved with the other preferences. Document contents,
command ids, CLI/MCP responses and file names do not change. **Preferences › Date format ›
Language › ქართული** writes Georgian month and weekday names (`crates/engine/src/dates.rs`).

`crates/ui-egui/src/i18n/ka.tsv` has 2,578 entries: every key of the other bundled catalogs, so every
command, menu, All tools label, `tl!` literal, contextual label (calendar weekdays, certificate
fields) and engine wording. It is written from the meaning of the English labels; no proprietary
translation resources were consulted. Product, format and standard names (PDF, OCR, JavaScript,
Word, PowerPoint, SHA-256, `+LOC`) stay as they are. Strings added later fall back to English until
they are translated.

## Georgian script

- **No case.** Mkhedruli has no upper and lower case. Section labels and comment initials go
  through `i18n::upper`, which leaves Georgian letters alone: plain `to_uppercase()` would turn
  them into Mtavruli, a display style that is not used in interface text. The catalog test rejects
  any Mtavruli character.
- **Count-neutral numerals.** A Georgian noun stays singular after a number (`5 გვერდი`), so the
  catalog has one plural form (`plural_none`). Fixed-count messages use a label and a colon
  (`გვერდი: {n}`, `ტეგები: {n}`), which reads correctly for any number.
- **Fonts.** None of the embedded faces (Inter, JetBrains Mono) or craft-fonts has Georgian. The
  desktop app draws it with one installed system face (SF Georgian or Arial Unicode on macOS,
  Sylfaen or Segoe UI on Windows, Noto Sans Georgian or DejaVu Sans on Linux), placed right after
  the app's own face in every family (`theme::GEORGIAN_FALLBACK`). The web build has no installed
  fonts, so Georgian shows replacement boxes there until craft-fonts gains an openly licensed
  Georgian face. `PDFCRAFT_SYSTEM_FONTS=0` turns the fallback off.
- **Dates.** Georgian month names (`იანვარი` … `დეკემბერი`), short months (`იან` …), weekdays and
  short weekdays (`კვ`, `ორ`, `სამ` …). Writing Georgian into a PDF (Fill & Sign, Edit text) is not
  supported yet: those paths write Windows-1252 only, and the app says so.

## Terminology

| Source | Georgian |
|---|---|
| File / Edit / View | ფაილი / რედაქტირება / ჩვენება |
| All tools / Tools | ყველა ხელსაწყო / ხელსაწყოები |
| Preferences… / Interface language | პარამეტრები… / ინტერფეისის ენა |
| Save as… | შენახვა როგორც… |
| Undo / Redo | დაბრუნება / ხელახლა შესრულება |
| Find… / Print… | ძებნა… / დაბეჭდვა… |
| Zoom in / Zoom out | გადიდება / დაპატარავება |
| Organize pages / Insert pages / Extract pages | გვერდების ორგანიზება / გვერდების ჩასმა / გვერდების ამოღება |
| Rotate pages | გვერდების შემობრუნება |
| Combine files | ფაილების გაერთიანება |
| Edit text / Add text | ტექსტის რედაქტირება / ტექსტის დამატება |
| Comments / Sticky note / Highlight | კომენტარები / შენიშვნა / მარკერი |
| Bookmarks / Link | სანიშნეები / ბმული |
| Fill & Sign / Signature / Certificates | შევსება და ხელმოწერა / ხელმოწერა / სერტიფიკატები |
| Prepare a form | ფორმის მომზადება |
| Redact | დაფარვა |
| Recognize text | ტექსტის ამოცნობა |
| Protect | დაცვა |
| Reduce file size | ფაილის ზომის შემცირება |
| Sanitize document | დოკუმენტის გაწმენდა |
| Accessibility / Tags | ხელმისაწვდომობა / ტეგები |
| Export to Word… | ექსპორტი Word-ში… |
| Clipboard / Paste | გაცვლის ბუფერი / ჩასმა |
| Tab (document tab) | ჩანართი |
| Page {n} of {total} | გვერდი {n} / {total} |

Please follow this table when adding strings. Commands use the verbal noun (`გვერდების შემობრუნება`),
not the imperative, and no label uses Mtavruli capitals.

## Maintenance

Add a row to `ka.tsv` (`context <TAB> English <TAB> Georgian`, format in the header of `ja.tsv`) for
each new English label. Keep placeholders (`{n}`, `{name}`, `{}`), shortcuts (`⌘G`), `…` and `\n`
exactly. `cargo test -p pdfcraft-ui-egui --lib i18n::tests::georgian_is_registered_and_complete`
fails on any command, menu, tool, `tl!` literal or engine wording without a Georgian row, on a row
left in English, on a lost `{}` hole and on Mtavruli; `upper_leaves_georgian_uncapitalised` covers
the casing guard, and `tests/fonts.rs` (`georgian_ui_uses_an_installed_georgian_face`) checks that
every Georgian letter of the catalog draws when a Georgian system face is installed.

A native-speaker skim of the catalog is welcome.

## Revision history

| Date | Change | Summary |
|---|---|---|
| 2026-10-13 | minor | Synced with main: 165 new strings (Tags panel, Copy Image, reading colours, browser printing, Scan tab, calendar and certificate contexts), 2 obsolete rows removed; 2,578 entries |
| 2026-10-12 | minor | Georgian catalog (2,415 entries), dates, system font fallback and casing guard |
