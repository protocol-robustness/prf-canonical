# PRF canonical codec

Status: standalone source candidate. Package identity is `prf-canonical`
`0.1.0-rc.1`; it is not published to a package registry.

This package contains the generic canonical value codec and domain-separated
SHA-256 implementations in Clojure and Rust, plus Clojure projections,
intent-contract hashing/equality, sequence encoding/commitment, and canonical
SHA-256 reference formatting. Artifact domain tags, canonical artifact
schemas, intent definitions, domain projections, and semantic admission rules
remain owned by their domain packages. Keyword domains resolve through an
owner-supplied mapping; intent hashing accepts an owner-supplied contract.

Public Clojure API:

- `prf.canonical.codec`: `canonical-bytes`, `canonical-bytes-hex`, and
  `validate-canonical-value!`.
- `prf.canonical.hash`: `hash-with-domain`, `domain-hash`,
  `resolve-domain-tag`, `project-canonical-safe`,
  `project-committable-content`, `hash-with-intent`, and `intent-hash=`.
- `prf.canonical.sequence`: `canonical-sequence-bytes`, `sequence-hash`, and
  `verify-sequence-commitment`.
- `prf.canonical.reference`: `sha256-ref`, `valid-sha256-ref?`, and
  `parse-sha256-ref`.

Keyword-to-domain and intent definitions remain with their semantic owners.
Owners pass a keyword mapping to `domain-hash`, or pass an intent contract
containing `:hash/intent`, string `:intent/domain-tag`, and optional
`:intent/projection-fn` to `hash-with-intent`. The PRF compatibility adapter
continues to resolve legacy keyword intents through PRF's registry and then
calls this owner-supplied API. Unsupported domains, intents, and hash schemes
fail explicitly. Sequence verification uses the decoder in this package;
PRF's adapter supplies its existing configurable resource limits explicitly.
The standalone package therefore has no dependency on PRF's source classpath,
configuration, domain registry, or intent registry.

The binary contract is specified in
[`CANONICAL_HASH_SPEC_V1_BINARY_ENCODING_ABI.md`](CANONICAL_HASH_SPEC_V1_BINARY_ENCODING_ABI.md).
Both implementations consume the same package-owned vectors at
`clj/test/resources/canonical-v1-vectors.tsv`.

## Verify locally

Run the Clojure conformance tests from `clj/` (standalone, with no PRF source
classpath):

```sh
clojure -M:test -m prf.canonical.test-runner
```

Run Rust conformance tests from the repository root:

```sh
cargo test --workspace --locked --offline
```

## Candidate bundle

## Provenance

This repository extracts the byte-identical `packages/canonical` source
snapshot from PRF integration revision
`0d443ef6387b6e4229c6692d7b4dcab186cade0c` (source tree
`114d2a32d25c7d0bcf523ca56e83082631ffb05a`). Its original candidate bundle
was `prf-canonical-0.1.0-rc.1.tar.gz`, SHA-256
`692d68d5e0f99450ae8da1bc857eb477f0d64e25550eeadff9646bf988d92aa6`.
The Clojure and Rust implementations and the shared frozen vectors are kept
in this repository. Canonical schemas, domain tags and artifact identities
remain owned by their domain packages.
