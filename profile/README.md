# ZK Experiments

Zero-knowledge experiments: tooling for [Noir](https://noir-lang.org) circuits and [Barretenberg](https://github.com/AztecProtocol/aztec-packages/tree/master/barretenberg) proofs, and applications built on it. The first is private verification of electronic identity documents: a phone proves that an ePassport or ID card was issued by a genuine national authority, and reveals nothing else unless its holder chooses to.

## Projects

| | |
|---|---|
| [**noir-zk**](https://github.com/zk-experiments/noir-zk) | Rust tooling and library for Noir circuits: frozen, versioned circuit registries and circuit packs, typed bindings generated from them, layered registries whose circuits generic kernels fold into one pipeline proof, and UltraHonk and Chonk proving and verification through Barretenberg. On [crates.io](https://crates.io/crates/noir-zk-backend). |
| [**csca-registry**](https://github.com/zk-experiments/csca-registry) | The registry the identity circuits trust: country signing CA certificates from the ICAO and national master lists and their CRLs, each signature-checked, committed in a Poseidon2 Merkle tree; revocations are permanent across releases. Refreshed nightly: [registry.zk-eid.dev](https://registry.zk-eid.dev/latest/registry.json). |
| [**eid-circuits**](https://github.com/zk-experiments/eid-circuits) | The identity layer: Noir circuits for ePassports and ID cards (ICAO 9303) that check a document's signature chain up to a registered country signing CA and its validity on a date, and commit to its data (DG1, the MRZ) for another layer to seal. Frozen as a noir-zk layer; circuit packs: [circuits.zk-eid.dev](https://circuits.zk-eid.dev/catalog.json). |
| [**zk-encryption**](https://github.com/zk-experiments/zk-encryption) | The encryption layer: a post-quantum channel for ZK pipelines, a pairwise handshake (Grumpkin DH plus a circuit-friendly lattice encryption over the receiver's ML-KEM-768 key) and a Poseidon2 ratchet, payload envelopes sealed inside the proof, and the Rust library that seals and opens them. Frozen as a noir-zk layer; circuits: [circuits.zk-experiments.dev](https://circuits.zk-experiments.dev/zk-encryption/latest/catalog.json). |
| [**emit-devnet**](https://github.com/zk-experiments/emit-devnet) | Both layers in one application: a reth node whose EVM verifies folded proofs, a private note pool, and a console wallet. A holder registers a passport once; every transfer then proves membership, the 2-in / 2-out payment and the sender's identity sealed to the receiver, in one 40 KB proof. |

## How they fit together

csca-registry publishes which signing authorities are genuine. eid-circuits proves a document chains to one of them and commits to its data without revealing which document it is. zk-encryption seals what a proof discloses to one recipient, post-quantum, inside the same proof. noir-zk freezes each layer's circuits, generates their Rust bindings, and folds circuits from several layers into one pipeline proof, on servers and on phones. emit-devnet combines the identity and encryption layers with a private transfer on a local chain.

Everything downloaded (registry files, circuit packs) is pinned by hash in code or checked against a published root: the hosts are mirrors, not trust anchors.
