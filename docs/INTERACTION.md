# v0.1 interaction model

## Status

This document records the accepted phone-control, clipboard, and power-action experience for v0.1. It is product scope, not an input-injection implementation specification.

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

While an authorized remote browser connection is active, the agent monitors the machine clipboard so it can offer convenient machine-to-phone retrieval. Before it first displays clipboard content, the browser UI must explain this behavior and obtain a clear opt-in. The consent is persisted for that paired browser until the user changes it, revokes the phone, or clears browser data.

Homer keeps no clipboard history, does not log clipboard content, does not use it for analytics or any other purpose, and does not silently synchronize it in the background. Browser and operating-system permission behavior may still require an explicit user gesture to copy content to the phone clipboard.

The maximum text size and overflow behavior remain implementation bounds to specify before release.

## Power actions

Sleep, graceful shutdown, and restart are available only as named actions. Each requires an explicit confirmation in the phone UI. Restart and shutdown include a visible short countdown with cancellation; the exact duration is an implementation parameter to validate. Homer does not expose arbitrary commands or a terminal.

## Connected phones and control sessions

v0.1 manages one target machine. Multiple phones may be paired and independently revoked, but only one phone has an active control session at a time. A connection attempt while another phone controls the machine must clearly report that state and either be rejected or use an explicitly confirmed handoff; the exact handoff UX remains to be selected before implementation.
