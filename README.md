# Bomkeeper

Bomkeeper is an evidence archive for software bills of materials (SBOMs). It is
designed to preserve original SBOMs and their proof of integrity with a
reference to the shipped product release.

The repository is in the M0 foundation and validation milestone. It currently
contains project documentation and decisions; product code has not started.

## Project scope

The open core will include the archive format, CLI, storage adapters,
verifier, and GitHub Action. The commercial part will include the server,
product index, web UI, Dependency-Track integration, sharing, and audit log.
See [the vision](docs/VISION.md) and [the architecture decision records](docs/adr/).

## Licensing

| Part | License |
| --- | --- |
| Open core: archive format, CLI, storage adapters, verifier, GitHub Action | Apache-2.0 ([LICENSE](LICENSE)) |
| Commercial server and services | FSL-1.1-MIT ([LICENSE-COMMERCIAL](LICENSE-COMMERCIAL)) |

The commercial license grants the MIT license on the second anniversary of
each software release, as specified in FSL-1.1-MIT. The open core is
independent of the commercial part.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md), [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md),
and [SECURITY.md](SECURITY.md).

## Status

Bomkeeper supports the CRA's retention requirements; it does not make a product
or organization CRA-compliant.
