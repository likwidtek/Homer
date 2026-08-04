# v0.1 interaction model

## Status

This document records the accepted phone-control, clipboard, and power-action experience for v0.1. It is product scope, not an input-injection implementation specification.

## Pairing and trusted-network disclosure

The user starts pairing locally in Decky. A pairing QR remains valid for five minutes and accepts one phone request. After scanning, the phone and Decky show the same six-digit comparison code; the local user has two minutes to approve or reject it. No code is typed, and a failed or expired attempt requires the user to start a new local pairing window.

Decky first run and each new phone explain once that Homer is for a trusted home network, sensitive messages are encrypted after pairing, the browser interface itself is delivered over HTTP, and an active hostile network can defeat that protection. The user explicitly continues. The normal phone interface then keeps only a compact trusted-LAN status link.

The connection begins through the machine’s current private address and automatically moves to its stable `.local` identity when supported. If local discovery is blocked, the phone remains usable through the current address and clearly explains that an address change may require locally approved reconnection. Users never type or configure a static address.

## Keyboard

- The phone uses its native on-screen keyboard to enter text into Homer’s text field. Homer sends entered text to the machine as the user types.
- Pasting text from the phone into that field sends the pasted text to the machine as well.
- Homer provides common keys that a phone keyboard does not normally expose, including Escape, Tab, Enter, Backspace, Delete, arrow keys, and function keys.
- Homer does not synthesize held keys or automatic key repeats. Normal text may of course contain repeated characters entered by the user.
- The intended v0.1 experience is input in Gaming Mode and Desktop Mode, both in and out of a game. Supported Bazzite and SteamOS configurations must validate that claim before release.

The precise special-key roster and any one-shot modifier/chord interaction need a small final UI design decision before implementation; they must not add held-key behavior.

## Trackpad and mouse

The browser control surface provides a relative trackpad with:

- left, middle, and right click;
- one-finger movement;
- two-finger scrolling; and
- two-finger tap for right click.

v0.1 excludes pinch/zoom, drag lock, sensitivity controls, additional gestures, and gamepad emulation.

## Clipboard

Clipboard transfer is text-only. Phone-to-machine transfer is explicit through the text field or paste action.

While an authorized remote browser connection is active, the agent monitors the machine clipboard so it can offer convenient machine-to-phone retrieval. Before it first displays clipboard content, the browser UI must explain this behavior and obtain a clear opt-in. The consent is persisted for that paired browser until the user changes it, revokes the phone, or clears browser site data. Clearing only downloaded page assets must not remove the consent or paired-phone credential.

Homer keeps no clipboard history, does not log clipboard content, does not use it for analytics or any other purpose, and does not silently synchronize it in the background. Browser and operating-system permission behavior may still require an explicit user gesture to copy content to the phone clipboard.

The maximum text size and overflow behavior remain implementation bounds to specify before release.

## Power actions

Sleep, graceful shutdown, and restart are available only as named actions. Each requires an explicit confirmation in the phone UI. Restart and shutdown include a visible short countdown with cancellation; the exact duration is an implementation parameter to validate. Homer does not expose arbitrary commands or a terminal.

## Connected phones and control sessions

v0.1 manages one target machine. Multiple phones may be paired and independently revoked, but only one phone has an active control session at a time. The controlling phone holds a thirty-second renewable lease. Its authenticated reconnect may replace its stale prior connection immediately. Another phone is clearly denied control until the current phone releases it or the lease expires; v0.1 provides no remote kick-over or cross-phone handoff.

A user who clears or loses Homer’s browser site data must pair again locally. This creates a new phone record; Decky device management must make the inaccessible old record understandable and easy to revoke.
