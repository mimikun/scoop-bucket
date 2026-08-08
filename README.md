# scoop-bucket

My personal [scoop](https://scoop.sh/) bucket — things I install on Windows that no maintained
bucket covers.

```powershell
scoop bucket add mimikun https://github.com/mimikun/scoop-bucket
scoop install mimikun/udev-gothic-nf
```

Right now that is Japanese programming fonts. Anything else with the same problem can go here
too.

## Why this exists

These fonts were installed by hand, which put them outside `scoop update *`. They then aged
silently: UDEV Gothic sat at v2.1.0 while v2.2.0 was out, PlemolJP at v2.0.4 while v3.0.0 was
out, and the Nerd Fonts glyphs bundled with them fell a full release behind — which surfaced
as a missing-glyph warning in WezTerm that took a session to diagnose.

The failure is silent, so the fix has to be automatic. **The rule for adding anything here is
that it is not covered by a maintained bucket.** Duplicating a bucket that already works just
creates a second thing to keep current.

Fonts covered by [`nerd-fonts`](https://github.com/matthewjberger/scoop-nerd-fonts) are
therefore deliberately absent — install those from there:

```powershell
scoop bucket add nerd-fonts
scoop install nerd-fonts/NerdFontsSymbolsOnly
```

## Contents

### Japanese programming fonts

| Manifest | Upstream | Notes |
|---|---|---|
| `udev-gothic` / `-hs` / `-nf` | [yuru7/udev-gothic](https://github.com/yuru7/udev-gothic) | BIZ UD Gothic + JetBrains Mono |
| `cica` | [miiton/Cica](https://github.com/miiton/Cica) | bundled with emoji |
| `hackgen` / `-nf` | [yuru7/HackGen](https://github.com/yuru7/HackGen) | Hack + GenJyuu Gothic |
| `plemoljp` / `-hs` / `-nf` | [yuru7/PlemolJP](https://github.com/yuru7/PlemolJP) | IBM Plex Mono + IBM Plex Sans JP |
| `firge` / `-nf` | [yuru7/Firge](https://github.com/yuru7/Firge) | Fira Mono + GenShin Gothic |

`-nf` is the Nerd Fonts patched variant; `-hs` renders full-width spaces visibly.

## Staying current

[Excavator](https://github.com/ScoopInstaller/GithubActions) runs hourly, follows each
upstream's releases, and commits new versions and hashes on its own. **This is the point of
the bucket.** A manifest with `checkver`/`autoupdate` but no CI behind it goes stale exactly
like a hand-installed font does — that is how the `cica` manifest in another bucket ended up
pinned two patch versions behind its own upstream.

If the workflow ever stops running, this bucket is worse than nothing, because it looks
maintained. Check the Actions tab before trusting it.

## Note on `-Recurse`

The font installer scans for `*.ttf` recursively. These archives nest their fonts under a
versioned directory, and PlemolJP nests one level deeper still, so the usual non-recursive
scan copied from other font buckets installs zero files without failing.

## Licensing

The manifests in this repository are MIT licensed. They contain only URLs, checksums, and
install scripts — **no packaged software is redistributed here**; scoop downloads each archive
directly from its upstream release.

Every font packaged here is licensed under the [SIL Open Font License 1.1](https://scripts.sil.org/OFL)
by its own author. Their license terms, not this repository's, govern the fonts themselves.
