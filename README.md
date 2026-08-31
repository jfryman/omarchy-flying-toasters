# Flying Toasters

The After Dark screensaver, drawn in your terminal, wired into Omarchy's idle
cycle. Chrome toasters with glowing elements flap across a black screen,
trailed by the occasional slice of toast.

Sprite shapes and colours are modelled on the original 1989 Berkeley Systems
sprite sheet, sampled directly rather than eyeballed.

## Install

```bash
omarchy plugin add https://github.com/<you>/omarchy-flying-toasters.git --enable --yes
omarchy plugin disable omarchy.idle
omarchy restart shell
```

## Why it replaces `omarchy.idle`

Omarchy has no screensaver plugin kind, and the built-in idle service hardcodes
`omarchy-launch-screensaver` — `idle.screensaver` in `shell.json` sets *when* the
screensaver fires, not *what* runs. The only supported way to change what runs
is to supply your own idle service, so this plugin is a fork of `omarchy.idle`
with one line repointed at `bin/flying-toasters-screensaver`.

**Consequences worth knowing:**

- This plugin owns your **lock screen** timing as well as the screensaver.
  `idle.screensaver` and `idle.lock` in `shell.json` still work exactly as before.
- Being a fork, it does not pick up upstream fixes to idle or lock behaviour.
  After an Omarchy upgrade that touches the idle service, re-fork it:
  `omarchy plugin clone omarchy.idle`, then reapply the one-line change.
- Exactly one idle service should be enabled. Running both this and
  `omarchy.idle` will start two screensavers and two lock timers.

## Uninstall

```bash
omarchy plugin remove jfryman.flying-toasters --yes
omarchy plugin enable omarchy.idle
omarchy restart shell
```

Re-enabling `omarchy.idle` is the important step — without an idle service your
screen will never lock.

## Running it directly

`bin/flying-toasters-screensaver` opens it full screen on every monitor, the same
way the idle service does. Pass `force` to start it even when the screensaver is
toggled off. Any key or mouse movement dismisses it.

`bin/flying-toasters` is the animation itself and runs in any terminal. Tunables,
all via environment:

| Variable | Default | Meaning |
|---|---|---|
| `FLYING_TOASTERS_DELAY` | `0.07` | frame delay, seconds |
| `FLYING_TOASTERS_MAX` | `5` | maximum sprites on screen |
| `FLYING_TOASTERS_SPAWN` | `18` | spawn chance per frame, % |
| `FLYING_TOASTERS_TOAST` | `25` | chance a spawn is toast, % |
| `FLYING_TOASTERS_GRACE` | `15` | frames to ignore input at startup |

## Notes

- Needs a terminal that can do 24-bit colour and block-drawing glyphs. Alacritty,
  Foot, Ghostty and Kitty all qualify; the launcher refuses anything else.
- The animation forces a UTF-8 locale. Under a C locale bash counts string
  indices in bytes rather than characters, which shears every sprite.
- Like the stock screensaver, it exits when another window takes focus. An app
  that steals focus while you are away will end it early.

## Licence

MIT. The sprite designs derive from After Dark, © Berkeley Systems.
