# Security policy

Report security issues privately to the repository owner rather than opening a public issue.

Never include any of the following in a report attachment or public discussion:

- enrollment tokens;
- Cloudflare Access client IDs or secrets;
- node or worker bearer tokens;
- private runtime archives;
- configuration or logs containing credentials.

Installer assets are immutable per release. Verify their SHA-256 values using `installer-manifest.json` before execution. Clients do not contain a GitHub PAT and obtain private runtime updates only through the authenticated control-plane broker.
