# Local release staging

Release binaries and their checksum sidecars are staged locally in `.artifacts/`.
They are intentionally ignored by Git: the public release repository publishes
them as GitHub Release assets after an explicit verified release operation.

Before publishing, verify each binary against its adjacent `.sha256` file.
