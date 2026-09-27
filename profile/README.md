# ZK Experiments

Zero-knowledge experiments: tooling for [Noir](https://noir-lang.org) circuits and [Barretenberg](https://github.com/AztecProtocol/aztec-packages/tree/master/barretenberg) proofs, and applications built on it. The first is private verification of electronic identity documents: a phone proves that an ePassport or ID card was issued by a genuine national authority, and reveals nothing else unless its holder chooses to.

## Projects

| | |
|---|---|
| [**noir-zk**](https://github.com/zk-experiments/noir-zk) | Rust tooling and library for Noir circuits: typed bindings generated from compiled circuits, frozen and versioned circuit registries, circuit packs, and UltraHonk and Chonk proving and verification through Barretenberg. On [crates.io](https://crates.io/crates/noir-zk-backend). |
| [**eid-circuits**](https://github.com/zk-experiments/eid-circuits) | Noir circuits for ePassports and ID cards (ICAO 9303): they check the document's signature chain up to a registered country signing authority, fold the steps into one Chonk proof, and encrypt the document data to viewer keys. Circuit packs: [circuits.zk-eid.dev](https://circuits.zk-eid.dev/catalog.json). |
| [**csca-registry**](https://github.com/zk-experiments/csca-registry) | The registry those circuits trust: country signing CA certificates from the ICAO and national master lists and their CRLs, each signature-checked, committed in a Poseidon2 Merkle tree. Refreshed nightly: [registry.zk-eid.dev](https://registry.zk-eid.dev/latest/registry.json). |

## How they fit together

csca-registry publishes which signing authorities are genuine. eid-circuits proves a document chains to one of them without revealing which document it is. noir-zk freezes those circuits, generates their Rust bindings, and proves and verifies them, on servers and on phones.

Everything downloaded (registry files, circuit packs) is pinned by hash in code or checked against a published root: the hosts are mirrors, not trust anchors.
