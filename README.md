# Grok Bot quick dialer for Omarchy

Press **Super+Shift+Q** and a photo grid of your Grok Bot agents drops from the top of the screen.
Tap one (or press 1-9, or arrows + Enter) and it opens that agent's chat and starts a voice call.
A small call card then shows who you are talking to, with a timer, and slides away when the call ends.
Press Super+Shift+Q during a call to hang up. Esc or clicking away closes the grid.

Built and used on Omarchy 4.5 (Hyprland 0.56, Lua config).

## Files
- `grok-dialer` - the grid (Python, GTK3 + gtk-layer-shell). Agents with a photo on top, the rest in a bottom row.
- `grok-call-card` - the call card (Python, GTK3 + gtk-layer-shell).
- `grok-voice-toggle` - talks to the Grok Bot app over the Chrome DevTools protocol (Node 22+, built-in WebSocket):
  lists the agents from the sidebar (`--list`), opens one and presses Start voice chat (`--call <id>`),
  hangs up a live call, or opens the dialer.

## Needs
- Grok Bot started with DevTools on port 9335, e.g. add `--remote-debugging-port=9335` to its launcher.
- `python-gobject`, `gtk3`, `gtk-layer-shell`, `python-cairo`, `nodejs`, `notify-send`.

## Install
```bash
cp grok-dialer grok-call-card grok-voice-toggle ~/.local/bin/ && chmod +x ~/.local/bin/grok-*
```
Then in `~/.config/hypr/bindings.lua` (Omarchy 4.5 binds Super+Shift+Q to the calculator):
```lua
hl.unbind("SUPER + SHIFT + Q")
o.bind("SUPER + SHIFT + Q", "Grok voice dialer", "node " .. os.getenv("HOME") .. "/.local/bin/grok-voice-toggle")
```

## Notes
- It reads the Grok Bot window's own page (aria-labels "Start voice chat" and "Hang up"); a Grok Bot update that renames them needs a one-line fix.
- Nothing leaves your machine; it only clicks buttons in your own Grok Bot window.
