# Bundled assets and their licensing status

`style.css` claims the theme bundles no third-party **code** — that is accurate:
every PHP, CSS and JS file is authored for Yachtiva. **Photography is a
separate question**, and it is the one item that has to be resolved before the
theme is distributed publicly (wp.org theme review and ThemeForest both require
every bundled asset to be GPL-compatible or explicitly licensed, and both ask
for image credits).

## Status: UNRESOLVED — the demo fleet photography

The 30 files in `assets/images/demo/` are the sample galleries the demo importer
loads into the Media Library (`inc/demo-import/importer.php`; the yacht →
filename map lives in `inc/demo-import/sample-data.php`).

What is verified:

* Each file is a JPEG around 1536×1024 (one exception: `ocean-breeze.jpg` is
  1672×941), re-encoded by PHP GD
  (`CREATOR: gd-jpeg v1.0 (using IJG JPEG v80), quality = 84`). **No EXIF, IPTC,
  XMP or C2PA metadata survives**, so the photographer, the creating tool and
  any embedded rights statement are gone — provenance cannot be recovered from
  the files themselves.
* They are **byte-identical** to the booking plugin's own shipped set
  (`plugins/yacht-booking-system/assets/demo/`, same MD5s), so this is *one*
  licensing question covering both products, not two.
* Neither repository records a source: no credits/licence file, no readme
  entry, and nothing in the plugin commits that introduced them
  (`66c4464`, `2f37024`, both 2026-09-21).

Nothing on record proves the images may be redistributed. Treat them as
unlicensed until the table below is replaced with a real credit line.

## Resolution — pick one before public distribution

| Option | Action | Outcome |
|---|---|---|
| **A. Licence them** | Locate the purchase/source record and record it in the table below (source name, licence, URL, date). | Keeps the current look; needs the paperwork. |
| **B. Replace them** | Drop licence-clear photos into `assets/images/demo/` **keeping the existing filenames**, then regenerate the table and delete the "unresolved" banner above. | Works immediately — the importer resolves by filename, so no code changes. Artwork changes. |
| **C. Ship without demo photos** | Delete the 30 files; the demo import then reports "Yacht … needs at least five gallery images" on **Yachtiva → Setup**. | Zero licence risk; the demo import is visibly incomplete. |

Replacing the files is genuinely zero-code: `yachtiva_import_demo_yacht_gallery()`
sideloads `{slug}.jpg` plus its `-02…-05` siblings by name and reuses the
attachment on re-import, so a same-named replacement set is picked up as-is.

### Regenerating the inventory below

```bash
cd assets/images/demo
for f in $(ls *.jpg | sort); do
  php -r '$i = getimagesize( $argv[1] );
    printf( "| `%s` | %s | %d×%d | %s |\n", $argv[1],
      number_format( filesize( $argv[1] ) ), $i[0], $i[1],
      substr( md5_file( $argv[1] ), 0, 12 ) );' "$f"
done
```

## Inventory

| File | Bytes | Dimensions | MD5 (first 12) — *no source recorded* |
|---|---|---|---|
| `aegean-muse.jpg` | 325,926 | 1536×1024 | `2c00b15252da` |
| `aegean-muse-02.jpg` | 376,352 | 1536×1024 | `ba9cf18caa72` |
| `aegean-muse-03.jpg` | 395,966 | 1536×1024 | `d81902334f6d` |
| `aegean-muse-04.jpg` | 342,094 | 1536×1024 | `36a28706d0eb` |
| `aegean-muse-05.jpg` | 279,531 | 1536×1024 | `0d4344eb2f86` |
| `golden-mirage.jpg` | 337,182 | 1536×1024 | `d61dd417996c` |
| `golden-mirage-02.jpg` | 374,552 | 1536×1024 | `f2b9799f4913` |
| `golden-mirage-03.jpg` | 367,408 | 1536×1024 | `29a7a46dd7d3` |
| `golden-mirage-04.jpg` | 390,203 | 1536×1024 | `fcd23855f7f3` |
| `golden-mirage-05.jpg` | 332,444 | 1536×1024 | `d3dcc20804d3` |
| `island-serenade.jpg` | 367,528 | 1536×1024 | `f4648f628058` |
| `island-serenade-02.jpg` | 405,010 | 1536×1024 | `d2e3e14c0ec6` |
| `island-serenade-03.jpg` | 466,865 | 1536×1024 | `fd3a926cd605` |
| `island-serenade-04.jpg` | 342,115 | 1536×1024 | `0d4f17800cad` |
| `island-serenade-05.jpg` | 402,290 | 1536×1024 | `cf32baf26f86` |
| `ocean-breeze.jpg` | 303,002 | 1672×941 | `0b018cb3f0e4` |
| `ocean-breeze-02.jpg` | 313,137 | 1536×1024 | `e58c83004a80` |
| `ocean-breeze-03.jpg` | 383,732 | 1536×1024 | `97a0ce50110c` |
| `ocean-breeze-04.jpg` | 333,182 | 1536×1024 | `4132b908f80f` |
| `ocean-breeze-05.jpg` | 276,967 | 1536×1024 | `fd7f780466ff` |
| `sapphire-horizon.jpg` | 384,194 | 1536×1024 | `98007cfb838c` |
| `sapphire-horizon-02.jpg` | 400,673 | 1536×1024 | `3f0b99994918` |
| `sapphire-horizon-03.jpg` | 462,105 | 1536×1024 | `90103421d8f6` |
| `sapphire-horizon-04.jpg` | 414,127 | 1536×1024 | `88fe4eed3d5c` |
| `sapphire-horizon-05.jpg` | 339,170 | 1536×1024 | `236209357585` |
| `southern-star.jpg` | 300,379 | 1536×1024 | `504001b3e998` |
| `southern-star-02.jpg` | 351,330 | 1536×1024 | `08e7a1f9528e` |
| `southern-star-03.jpg` | 393,440 | 1536×1024 | `224d6f83a2c3` |
| `southern-star-04.jpg` | 330,510 | 1536×1024 | `263d2aef151b` |
| `southern-star-05.jpg` | 268,297 | 1536×1024 | `c917c7f1c0b1` |

The `{slug}.jpg` file of each group is the yacht's featured image and the first
gallery item; `-02` … `-05` complete the five-photo gallery.

## Other bundled assets

| Asset | Status |
|---|---|
| `screenshot.png` (1200×900, 549,754 bytes) | A rendering of the theme's own pages — theme artwork, no third-party rights to clear. Refresh it when the design changes. |
| `assets/css/*.css`, `assets/js/*.js` | Authored for this theme; no third-party library is bundled. `assets/js/testimonial-block.js` uses only WordPress-provided packages (`wp-blocks`, `wp-element`, …). |
| Fonts | **Not bundled.** Playfair Display and Inter are requested from Google Fonts at runtime; if a local `assets/fonts/yachtiva-fonts.css` is added, the site owner supplies those faces (see `docs/theme-settings.md`). |
| `languages/yachtiva.pot` | Generated from this theme's own strings with `wp i18n make-pot`. |

## Pre-distribution checklist

1. The resolution above has been decided, and this inventory either carries real
   credits (**A**) or the files have been swapped (**B**) / removed (**C**). Note
   the same question applies to the plugin's `assets/demo/` set, since the files
   are identical.
2. `wp i18n make-pot . languages/yachtiva.pot --domain=yachtiva` runs without
   warnings (it audits for missing `translators:` comments).
3. Every PHP file passes `php -l`, and `wp-content/debug.log` stays empty while
   browsing the homepage, the fleet archive, a single yacht, search, a 404 and a
   commented post.
4. `readme.txt`'s changelog has an entry for the version declared in `style.css`.
