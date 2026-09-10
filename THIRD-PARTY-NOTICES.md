# Third-party notices

`index.html` bundles one third-party library. Its notice is reproduced below and
must be retained in any redistribution.

## JSZip 3.10.1

Used to read and write the SCORM `.zip` archive inside the browser.

```
JSZip v3.10.1 - A JavaScript class for generating and reading zip files
<http://stuartk.com/jszip>

(c) 2009-2016 Stuart Knightley <stuart [at] stuartk.com>
Dual licenced under the MIT license or GPLv3.
See https://raw.github.com/Stuk/jszip/main/LICENSE.markdown

JSZip uses the library pako released under the MIT license:
https://github.com/nodeca/pako/blob/main/LICENSE
```

This project uses JSZip under the MIT option. The library source is inlined in
`index.html` rather than loaded from a CDN, so the page has no external script
dependency and works offline.

## Fonts

The interface requests IBM Plex Sans, Serif and Mono from Google Fonts. The
fonts are not bundled. If the request is blocked or unavailable the interface
falls back to system fonts and remains fully usable. See the README if you need
to remove the request entirely.
