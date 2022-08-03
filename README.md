# HackSlash

Variant of the [Hack typeface](https://github.com/source-foundry/Hack) with forward slashed zero (u0030-forwardslash) [alternate glyphs](https://github.com/source-foundry/alt-hack).

<p float="left" align="center">
<img src="https://user-images.githubusercontent.com/4249591/31325182-35ab6c76-ac88-11e7-906e-6fd295364f10.png" height="60">
<img src="https://user-images.githubusercontent.com/4249591/31325211-7ea992ae-ac88-11e7-987b-1037e8275dca.png" height="60">
<img src="https://user-images.githubusercontent.com/4249591/31325225-9677d95e-ac88-11e7-9777-3687faa2d9a0.png" height="60">
<img src="https://user-images.githubusercontent.com/4249591/31325238-ae1b6f08-ac88-11e7-97bd-636d2840f52f.png" height="60">
</p>

## Release

Release zip files provide two sets of ttf files:

- `HackSlash-*.ttf` (`HackSlash.*.zip`) have the new font family "HackSlash", so are co-installable with the original Hack ttf files.
    
- `Hack-*.ttf` (`Hack.*.zip`) have the original font family "Hack", so are a drop-in replacement for the original Hack ttf files.

You can also find the ttf files in [build/ttf](build/ttf).

## Notes

Built on Debian/sid by following the instructions for the [alt-hack glyphs](https://github.com/source-foundry/alt-hack). The following additional steps were needed.

### Rename glyphs in ttf hinting

Recent fontmake renames glyphs so it is necessary to also rename glyphs from unicode to symbolic names in ttf hinting. See [Hack#500](https://github.com/source-foundry/Hack/issues/500):
```
perl -pi -e "s/^uni0021/exclam/;s/^uni0023/numbersign/;s/^uni0025/percent/;s/^uni002B/plus/;s/^uni0030/zero/;s/^uni0038/eight/;" postbuild_processing/tt-hinting/*.txt
```

### Add wrapper for python executable

Scripts invoke `python` but Debian/sid has `python3`. Fix with a wrapper `python` script (`chmod +x python`) somewhere on your path:
```
#!/bin/bash
exec python3 "$@"
```

### Set tool path when building ttf files

Tools are installed in `$HOME/.local/bin` so prepend it to your path when invoking `build-ttf.sh`:
```
PATH=$HOME/.local/bin:$PATH ./build-ttf.sh --install-dependencies
```

### Create fonts with HackSlash font family and zip font files

Install [fontname.py](https://github.com/chrissimpkins/fontname.py) on your path then create `HackSlash-*.ttf` font files with "HackSlash" font family and zip files with:
```
cd build/ttf
for f in Hack-*.ttf; do cp $f ${f/Hack/HackSlash}; done
for f in HackSlash-*.ttf; do fontname.py HackSlash $f; done
zip HackSlash.$(date -I).zip HackSlash-*.ttf
zip Hack.$(date -I).zip Hack-*.ttf
```

## License

**Hack** work is &copy; 2018 Source Foundry Authors. MIT License

**Bitstream Vera Sans Mono** &copy; 2003 Bitstream, Inc. (with Reserved Font Names _Bitstream_ and _Vera_). Bitstream Vera License.

The font binaries are released under a license that permits unlimited print, desktop, web, and software embedding use for commercial and non-commercial applications.

See [LICENSE.md](https://github.com/source-foundry/Hack/blob/master/LICENSE.md) for the full texts of the licenses.
