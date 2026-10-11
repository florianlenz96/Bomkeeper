# Contributing

Contributions are welcome. Before proposing a change, please read
[the vision](docs/VISION.md) and the relevant
[architecture decision records](docs/adr/).

## Pull requests

- Open or identify an issue for the change and keep each pull request focused
  on one issue.
- Use English for code, comments, commit messages, and public documentation.
- Add tests with product changes. Storage adapter tests must use MinIO or
  Azurite, never production storage accounts.
- Record major technical decisions, format changes, new storage backends, and
  security-sensitive decisions as ADRs.
- Do not add dependencies with GPL or AGPL licenses to the open core.
- Do not modify or delete archived bundles. Preserve SBOMs byte-for-byte and
  calculate hashes from the original bytes.

There is no product build or test command yet because the repository currently
contains documentation only. The project will document those commands when
product code is introduced.

## License

By submitting a contribution, you agree that it will be distributed under the
license applicable to the part of the project it modifies, as described in
[README.md](README.md). Contributions to the open core are under Apache-2.0.
