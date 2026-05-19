# Bitcoin v0.1.0 P2P Protocol — Definitive Audit

Source code verification of all P2P network commands in Bitcoin v0.1.0 (Satoshi Nakamoto, January 9, 2009).

Repository mirror: [0xMagnuz/Bitcoin-v0.1](https://github.com/0xMagnuz/Bitcoin-v0.1)

## Key Finding

Bitcoin v0.1.0 contains **16 P2P network commands (15 unique)** — not the 7-8 typically documented. Four marketplace commands (review, checkorder, submitorder, reply) and four publish/subscribe commands (subscribe, sub-cancel, publish, pub-cancel) have been systematically omitted from virtually all modern Bitcoin protocol documentation for over 17 years.

## What's in the code

**12 ProcessMessage() handlers** (src/main.cpp):

| # | Command | Line | Category |
|---|---------|------|----------|
| 1 | version | 1705 | Core |
| 2 | addr | 1748 | Core |
| 3 | inv | 1772 | Core |
| 4 | getdata | 1795 | Core |
| 5 | getblocks | 1832 | Core |
| 6 | tx | 1868 | Core |
| 7 | review | 1921 | Marketplace |
| 8 | block | 1939 | Core |
| 9 | getaddr | 1955 | Core |
| 10 | checkorder | 1974 | Marketplace |
| 11 | submitorder | 1993 | Marketplace |
| 12 | reply | 2015 | Marketplace |

**4 PushMessage() pub/sub commands** (src/net.h, src/net.cpp):

| # | Command | Source | Line |
|---|---------|--------|------|
| 1 | subscribe | net.cpp | 238 |
| 2 | sub-cancel | net.cpp | 260 |
| 3 | publish | net.h | 831 |
| 4 | pub-cancel | net.h | 842 |

## What's NOT in v0.1.0

- **verack** — 0 matches. Added in v0.2.0
- **ping** — Only IRC PING. Added as Bitcoin P2P command in v0.6.0
- **alert** — 0 matches. Added in v0.3.11

## Verification

```bash
git clone https://github.com/0xMagnuz/Bitcoin-v0.1
cd Bitcoin-v0.1
grep 'if (strCommand ==' src/main.cpp     # 12 handlers
grep 'PushMessage("' src/net.h src/net.cpp # pub/sub commands
grep -r 'verack' src/                      # 0 results
grep -r '"ping"' src/                      # 0 results
```

## Files

- **Bitcoin_v01_Definitive_Audit.pdf** — Full audit report with line numbers, tables, and methodology
- **bitcointalk_post.txt** — Publication text for Bitcointalk
- **bitcoin-dev_email.txt** — Publication text for bitcoin-dev mailing list

## License

This audit research is released into the public domain. The Bitcoin v0.1.0 source code is Copyright (c) 2009 Satoshi Nakamoto under the MIT License.
