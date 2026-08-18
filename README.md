# HAN Meter CRC Tool

A single-page tool for checking the CRC on HAN meter P1 telegrams. Paste a telegram, get a verdict — no install, no dependencies, no data leaves the browser.

Open [`han_crc_tool.html`](han_crc_tool.html) in any browser.

## What it does

1. **Reconstruct the telegram** — paste either a parsed JSON array of lines (`["/LGF5E360", "0-0:1.0.0(...)", ..., "!AA92"]`) or the raw lines one per line. Choose line endings (CRLF or LF) and whether there's an empty line after the header.
2. **Compute CRC16/IBM** — poly `0x8005`, init `0x0000`, reflected in and out, no final XOR. The result is compared against the `!` checksum in the telegram.
3. **Try all combinations** — if the checksum doesn't match, this sweeps the twelve plausible variants (CRLF/LF × empty line or not × CRC16/IBM, CRC16/CCITT with init `0x0000`, and CRC16/CCITT-FALSE) and tells you which one the meter actually used.

## Why

P1 telegrams fail CRC checks for boring reasons — a stripped empty line, LF where the meter sent CRLF, a logger that normalized the line endings on the way through. Step 3 exists so you can find out which of those it was instead of guessing.
