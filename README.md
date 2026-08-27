# bedit-theme-dracula

A [Dracula](https://draculatheme.com) colour theme for the
[bedit](https://github.com/broodlang/bedit) editor — a **plugin package** for
[Brood](https://broodlang.org)'s hive registry.

It's the reference example of a bedit theme plugin: pure data, no dependency on
the editor itself. The package declares `:enhances {bedit ">= 0.1"}` in its
manifest (Debian's `Enhances:` — "I add functionality to bedit", a soft signal,
never a hard dependency), and its entry module exposes a single `(bedit-plugin)`
function returning a contribution map. bedit discovers installed `:enhances bedit`
packages on startup and folds each one's `:themes` into `M-x theme-select`.

## Install

In a bedit checkout:

```bash
nest add bedit-theme-dracula :version "^0.3"
```

Reopen the editor — **Dracula** appears in `M-x theme-select`, registered on
start. (Discover other bedit plugins with `nest search --enhances bedit`.)

## The contribution contract

A theme plugin never calls into bedit — it only *describes* what it offers, and
the host decides how to apply it. That's what lets it `:enhances bedit` without
depending on it (which would be circular):

```brood
(defn bedit-plugin ()
  {:themes [{:name "Dracula" :palette { … 16 role → "#rrggbb" … }}]})
```

The palette maps bedit's 16 semantic role names (`:base`, `:text`, `:blue`,
`:red`, …) to hex colours — the same shape as the editor's bundled themes. The
`:themes` value is a vector, so a single plugin can ship several palettes; the
contribution map is open, so future kinds (`:keymaps`, `:modes`) slot in beside
it.

## Develop

```bash
nest test            # the contribution-shape contract
```

## Licence

AGPL-3.0-or-later, matching bedit and Brood.
