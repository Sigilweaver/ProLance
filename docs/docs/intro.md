---
slug: /
title: Introduction
---

# ProLance

ProLance is a columnar, memory-mapped mass-spectrometry store built on
[Lance](https://lancedb.github.io/lance/). It ingests vendor formats
(via [OpenProteo](https://github.com/Sigilweaver/OpenProteo)) or mzML
and exposes a queryable store from both Rust and Python with direct
PyArrow / Polars / Pandas integration.

This is the 0.2.0-alpha release. APIs and the on-disk schema may break
without notice until 1.0.

## What is in the box

| Component        | Purpose                                                       |
| ---------------- | ------------------------------------------------------------- |
| `prolance-core`  | Lance store, schema, scalar indexes, range-query API.         |
| `prolance-ms`    | mzML reader/writer, streaming ingest, vendor feature gates.   |
| `prolance-cli`   | `prolance` binary: ingest, inspect, query, export back to mzML. |
| `prolance` (PyPI) | Python bindings (PyO3) backed by `prolance-core` + `prolance-ms`. |

## Stack position

ProLance sits at the storage layer of the OpenProteo stack:

```
vendor file (Thermo .raw, Bruker .d/, Waters .raw/, or .mzML)
   |
   v
openproteo-io  (vendor parsing, all of it - ProLance never touches
   |            vendor formats directly)
   v
prolance-ms / prolance-core  (Lance dataset, indexed by RT and m/z)
   |
   v
queries from Rust, Python, or the CLI
```

## Quickstart

The CLI is the fastest way to try it out:

```bash
cargo install --git https://github.com/Sigilweaver/ProLance prolance-cli
prolance ingest path/to/spectra.mzML --store ./run-store
prolance query ./run-store --rt 30..35 --mz 500..510
```

Python:

```bash
pip install prolance
```

```python
from prolance import Store
store = Store.open("./run-store")
batch = store.query(rt=(30, 35), mz=(500, 510))
```

See [GitHub](https://github.com/Sigilweaver/ProLance) for the current
roadmap and contribution guidelines.
