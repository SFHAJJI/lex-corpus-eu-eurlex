# lex-corpus-eu-eurlex

**Consolidated EU law with its dates — and its text.** Every consolidated
version of a flagship set of EU acts, with publisher-asserted validity
intervals and the verbatim consolidated XHTML as retrieved from the Publications
Office's official dissemination channel (Cellar).

**8 works · 46 consolidated versions**, including future-dated ones (the CRR
alone has 22 states through 2026-06-26 — served with a `provisional` marker).

**[Per-article dataset](https://github.com/SFHAJJI/lex-articles)** ·
**[Live demo](https://law.soufien.lu)** ·
**[MCP endpoint](https://law.soufien.lu/mcp)** ·
**[Engine](https://github.com/SFHAJJI/lex)**

## Why a git repository?

`git log` is the legislative history, `git diff` is "what changed between two
consolidations", and a clone is a complete, tamper-evident copy — every byte
covered by a sha256 recorded inside the content itself. No API, no account, no
database server: the format outlives any website built on top of it.

| Work | CELEX | Versions |
|---|---|---|
| GDPR | 32016R0679 | 1 |
| DORA | 32022R2554 | 1 |
| AI Act | 32024R1689 | 1 |
| NIS2 | 32022L2555 | 1 |
| MiFID II | 32014L0065 | 14 |
| CRR | 32013R0575 | 22 |
| PSD2 | 32015L2366 | 3 |
| SFDR | 32019R2088 | 3 |

## ⚠ Consolidated text has no legal effect

Only acts published in the Official Journal are authentic. Every version here
links to its EUR-Lex page; consolidations are working documents. Lex serves
them with that statement attached (spec §9.6) — always the timeline and the
text, never an interpretation.

## Layout

```
works/32013r0575/                       ← CRR
  meta.json
  versions/
    2013-06-28/
      meta.json                         ← validity, events, observations, sha256
      en.html                           ← verbatim XHTML as retrieved (hash covers this file)
      en.fmx4/                          ← Formex 4 manifestation, verbatim zip members
        CL2013R0575EN0000020.0001.xml   ← one sha256 observation per member (format: "fmx4")
    2015-01-18/…                        ← 22 dated states
```

`valid_from` is the publisher's consolidation date; `valid_to` derives from the
next consolidation in the publisher's own sequence. Bodies above 4 MB keep
metadata + link only (disclosed per-version as `text.available=false`,
`reason="not-fetched"`) — for those versions the Formex manifestation usually
still fits and carries the full text. Formex is fetched with an identity guard:
the file's own `INFO.CONSLEG START.DATE` must match the version date, or it is
not stored.

## The six intake answers (spec §1.5)

1. **What does it publish?** Regulations, directives, and their consolidated
   versions — https://eur-lex.europa.eu.
2. **Authority?** Binding EU instruments (regulations directly applicable,
   directives on transposition); consolidations are non-authentic working texts,
   per the publisher's own notice.
3. **Retrievable mechanically?** Yes — Cellar SPARQL + REST content negotiation
   (publications.europa.eu; robots.txt: `Allow: /` for these paths).
4. **May we republish the text?** **Yes** — reuse permitted with attribution,
   Commission Decision 2011/833/EU.
5. **May we republish the metadata?** Yes — same basis.
6. **Superseded versions retained?** Yes — Tier A; every consolidated version
   remains addressable by CELEX.

## Attribution

© European Union, 1998–2026 — reuse permitted with attribution (Commission
Decision 2011/833/EU). Bodies stored verbatim as retrieved; no text altered.
See [NOTICE](NOTICE). Demo + MCP server: https://github.com/SFHAJJI/lex
