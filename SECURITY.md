# Security

## Boundary

`rapp-cli` is a client for independently installed RAPP services. It does not
sandbox Brainstem agents: a `*_agent.py` loaded by Brainstem executes with the
Brainstem process user's privileges.

The CLI therefore:

- never imports agent Python itself;
- never modifies or embeds `rapp-installer`;
- refuses HTTP redirects and credential-bearing URLs;
- allows plaintext HTTP by default only on loopback;
- bounds JSON, stream, error, and agent payload sizes;
- accepts LAN secrets through environment variables or protected files, not
  command-line values;
- requires explicit confirmation for importing, installing, removing, or
  switching credentials;
- treats ring installers and static API command strings as data, never code;
- stops no process merely because it owns a port.

POSIX secret files must have mode `0600`. On Windows, the CLI rejects reparse
points but relies on the current account's filesystem ACLs; it does not attempt
to rewrite or certify those ACLs.

`rapp brainstem health`, `rapp doctor --deep`, and `rapp agent list` call
Brainstem routes that may import installed cartridges. Use the shallow
`rapp status` probe when inspecting an untrusted installation.

## Reporting

Report vulnerabilities privately to the repository owner. Do not include
tokens, LAN secrets, `.env` files, private agent source, diagnostic exports, or
personal paths in a public issue.
