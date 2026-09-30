# Flying Toasters

The After Dark screensaver, drawn in your terminal, wired into Omarchy's idle
cycle. Chrome toasters with glowing elements flap across a black screen,
trailed by the occasional slice of toast.

Sprite shapes and colours are modelled on the original 1989 Berkeley Systems
sprite sheet, sampled directly rather than eyeballed.

## Install

```bash
omarchy plugin add https://github.com/jfryman/omarchy-flying-toasters.git --enable --yes
omarchy restart shell
```

## How it works

This is a `screensaver`-kind plugin. Omarchy's idle service runs
`bin/flying-toasters-screensaver` in place of the built-in terminal screensaver,
the same way `bar.id` selects a bar plugin. Idle and lock timing stay with the
stock idle service — this plugin does not fork it and has no QML at all.

Requires the `screensaver` plugin kind, proposed upstream in
[omacom/omarchy#9493](https://github.com/omacom/omarchy/pull/9493). Until that
lands, the [`idle-service-fork`](https://github.com/jfryman/omarchy-flying-toasters/tree/idle-service-fork)
branch of this repo ships its own idle service instead; its README explains how
to install it.

> **On an unpatched Omarchy this plugin installs but does nothing.** The stock
> plugin validator skips kinds it does not recognise rather than rejecting them,
> so `omarchy plugin add` reports success and the shell then ignores the plugin.
> There is no error to tell you why the toasters never appear.

### Upgrading from the idle-service version

Early installs of this repo were the idle-service fork, installed alongside
`omarchy plugin disable omarchy.idle`. `omarchy plugin update` moves such an
install onto this version, which has no idle service of its own — so re-enable
the stock one, or your screen will never lock:

```bash
omarchy plugin enable omarchy.idle
omarchy restart shell
```

## Uninstall

```bash
omarchy plugin remove jfryman.flying-toasters --yes
omarchy restart shell
```

Removing the plugin clears `idle.screensaverId`, so the built-in screensaver
comes back. Lock behaviour is unaffected either way — the stock idle service
owned it the whole time.

## Running it directly

`bin/flying-toasters-screensaver` opens it full screen on every monitor, the same
way the idle service does: it hands off to `omarchy-launch-screensaver --exec`, so
each monitor gets its own screensaver workspace exactly like the built-in one.
Pass `force` to start it even when the screensaver is toggled off. Any key or
mouse movement dismisses it on every monitor at once.

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
