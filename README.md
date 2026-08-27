# Local AI Edge Client — official installers

This public repository is the binary-only distribution channel for the Local AI Edge Client.
The control plane and runtime source remain private. Git tags in this repository contain only documentation; installer binaries are attached as GitHub Release assets.

## Downloads

Download the asset matching your system from [the latest release](https://github.com/topolar/local-ai-services-v2-releases/releases/latest):

- `las-edge-installer-<version>-linux-x86_64.tar.gz`
- `las-edge-installer-<version>-macos-aarch64.tar.gz`
- `las-edge-installer-<version>-macos-x86_64.tar.gz`
- `las-edge-installer-<version>-windows-x86_64.zip`

Verify the asset against `installer-manifest.json` or its adjacent `.sha256` file before running it.

The installer asks interactively for:

1. a short-lived, one-time enrollment token;
2. the Cloudflare Access service client ID;
3. the Cloudflare Access service secret.

Secrets are written to private local files and are never passed in the command line. The installer then downloads the hash-pinned private runtime through the authenticated control-plane broker.

## Update behavior

The installed client receives update directives from the control plane. A normal update closes admission for new jobs, waits until the server reports zero active jobs, installs the verified candidate, and restarts. A force update stops immediately; durable leases and fencing remain authoritative. Failed candidates roll back to the previous release.

## Source and security

This repository intentionally does not contain the private runtime source. Do not open issues containing enrollment tokens, Cloudflare Access credentials, node tokens, logs with secrets, or private runtime bundles. See [SECURITY.md](SECURITY.md).
