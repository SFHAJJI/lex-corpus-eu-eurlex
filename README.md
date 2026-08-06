# lex-corpus-eu-eurlex

[![Reuse](https://img.shields.io/badge/reuse-EU%20notice-blue?style=flat-square)](NOTICE)
[![Source](https://img.shields.io/badge/source-EUR--Lex%20Cellar-003399?style=flat-square)](https://eur-lex.europa.eu)
[![Corpus](https://img.shields.io/badge/corpus-live%20coverage-brightgreen?style=flat-square)](https://law.soufien.lu/coverage)
[![Formats](https://img.shields.io/badge/formats-Formex%204%20%C2%B7%20XHTML-orange?style=flat-square)](README.md)
[![Live](https://img.shields.io/badge/live-law.soufien.lu-6f42c1?style=flat-square)](https://law.soufien.lu)

**EU law with its dates, relationships, and official text.** A reviewed,
configuration-led scope of binding EU law across the domains most useful to
Luxembourg practitioners. It stores every available official original and
consolidated expression in French and English, plus the amendment, corrigendum,
repeal, predecessor, successor, legal-basis, delegated, and implementing
relationships needed to explain the timeline.

The generated [manifest](manifest.json) and the
[live coverage page](https://law.soufien.lu/coverage) are the source of truth for
counts and date ranges. They are not copied into prose that becomes stale after
the next publisher run.

**[Per-article dataset](https://github.com/SFHAJJI/lex-articles)** ·
**[Live demo](https://law.soufien.lu)** ·
**[MCP endpoint](https://law.soufien.lu/mcp)** ·
**[Engine](https://github.com/SFHAJJI/lex)**

## Why a git repository?

A clone is a complete, tamper-evident copy, every byte covered by a sha256
recorded inside the content itself. No API, no account, no database server: the
format outlives any website built on top of it.

What git is **not** here is the timeline. A commit records when this corpus
observed a version, never when the act applied. Both time axes live inside the
hashed content, in each version's `meta.json`, never in mutable commit
metadata. The full argument, and what that choice cost, is at
[law.soufien.lu/decisions](https://law.soufien.lu/decisions).

The scope is expandable: adding another EUR-Lex subject domain for an existing
document class is a reviewed configuration change and backfill. A new publisher
or document class still requires an adapter implementation and its own evidence
tests. The exact selectors, exclusions, and closure rules are published in the
[engine repository](https://github.com/SFHAJJI/lex/blob/main/src/Lex.Sources.EurLex/eu-scope.json).

## ⚠ Consolidated text has no legal effect

Only acts published in the Official Journal are authentic. Every version here
links to its EUR-Lex page; consolidations are working documents. Lex serves
them with that statement attached (spec §9.6), always the timeline and the
text, never an interpretation.

## Layout

```
works/32013r0575/                       ← CRR
  meta.json
  versions/
    2013-06-28/
      meta.json                         ← validity, events, observations, sha256
      en.html                           ← verbatim XHTML when that channel is used
      en.fmx4/                          ← Formex 4 manifestation, verbatim zip members
        CL2013R0575EN0000020.0001.xml   ← one sha256 observation per member (format: "fmx4")
    2015-01-18/…                        ← 22 dated states
```

`valid_from` is the publisher's expression date; `valid_to` derives from the
next official expression in the publisher's own sequence. Bodies above 4 MB keep
metadata + link only (disclosed per-version as `text.available=false`,
`reason="not-fetched"`), for those versions the Formex manifestation usually
still fits and carries the full text. Formex is fetched with an identity guard:
the file's own `INFO.CONSLEG START.DATE` must match the version date, or it is
not stored.

## The six intake answers (spec §1.5)

1. **What does it publish?** Binding regulations, directives, decisions, primary
   law, and their available original and consolidated expressions,
   https://eur-lex.europa.eu.
2. **Authority?** Binding EU instruments (regulations directly applicable,
   directives on transposition); consolidations are non-authentic working texts,
   per the publisher's own notice.
3. **Retrievable mechanically?** Yes, Cellar SPARQL + REST content negotiation
   (publications.europa.eu; robots.txt: `Allow: /` for these paths).
4. **May we republish the text?** **Yes**, reuse permitted with attribution,
   Commission Decision 2011/833/EU.
5. **May we republish the metadata?** Yes, same basis.
6. **Superseded versions retained?** Yes, Tier A; every consolidated version
   remains addressable by CELEX.

## Attribution

© European Union, 1998-2026, reuse permitted with attribution (Commission
Decision 2011/833/EU). Bodies stored verbatim as retrieved; no text altered.
See [NOTICE](NOTICE). Demo + MCP server: https://github.com/SFHAJJI/lex

- Browse: any `works/<slug>/versions/<date>/meta.json`
- The lex-index/3 release contract covers local vectors, the pinned encoder and
  tokenizer, scope, and benchmark evidence. A canonical whole-artifact manifest
  binds every file by hash and size and is signed with a non-exportable Azure
  Key Vault P-256 key. The embedded index stamp remains public provenance, not
  the runtime trust root. The live rollout state is reported at
  [law.soufien.lu/verify](https://law.soufien.lu/verify).
- MCP server + web demo: https://github.com/SFHAJJI/lex

## How this repository stays current

One scheduled job runs nightly and drives every publisher in the fleet. There is
no manual publication step. GitHub Actions uses OIDC and short-lived Azure
authorization; the signing private key never leaves Key Vault.

1. **Ingest**, ask the publisher which versions exist, download any not seen
   before, write them *verbatim*. An existing body file is never reopened for
   writing: the evidence layer is append-only by construction.
2. **Anomaly gate**, if the work count drops by more than 5%, assume a partial
   upstream response, discard everything and **commit nothing**. A bad night
   leaves yesterday's good data in place rather than corrupting history.
3. **Derive**, regenerate the per-article layer in
   [lex-articles](https://github.com/SFHAJJI/lex-articles).
4. **Determinism guard**, if derived output changed while no source file did,
   the extractor is non-deterministic: fail loudly, **commit nothing**.
5. **Index & sign**, build lex-index/3, local vectors and benchmark evidence,
   sign the canonical artifact manifests, and verify every file before release.
6. **Deploy safely**, build an immutable image, start a zero-traffic Container
   App revision, exercise health, MCP, Luxembourg and EU search, then promote it.
7. **Report**, write a three-state outcome per publisher
   (`ran_committed` / `ran_no_change` / `failed_*`) and open an issue on failure.

Every index carries a provenance stamp recording **when it was built and from
which corpus commit**, and every MCP tool response returns it. Runtime trust is
reported separately from the pinned whole-artifact manifest. Live status:
<https://law.soufien.lu/built>

## Support

This is free and open, and it stays that way whatever you decide. It is also not free to run:
the live site, the nightly jobs and the storage sit on Azure infrastructure I pay for out of
pocket, and I maintain it on my own time.

If it saved you an afternoon, you can [buy me a coffee ☕](https://buymeacoffee.com/shajji)
and put it towards the hosting bill. Starring the repo helps just as much, and costs nothing.
