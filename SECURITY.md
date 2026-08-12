# Security policy

WP MCP hands an AI agent write access to a live WordPress site, so security is the product, not a feature of it. Reports are taken seriously and answered.

## Reporting a vulnerability

**Do not open a public issue.** Open a private security advisory instead:

https://github.com/wpmcp/wpmcp/security/advisories/new

Please include:

- The affected version, and whether it is the free build or the paid build
- The ability or endpoint involved
- A minimal reproduction, including the request or tool call and the arguments
- What an attacker gains, and what level of access they need to start

You will get an acknowledgement, and a fix or an explanation of why the behaviour is intended. Please give a reasonable window to ship a fix before disclosing publicly. Reporters are credited in the release notes unless they ask not to be.

## Supported versions

Only the latest release receives security fixes. Older versions are not patched. Update before reporting a problem you found on an older build.

## What is in scope

- Authentication and authorisation bypass: OAuth 2.1 flows, application password handling, bearer token handling, session or identity confusion
- Privilege escalation, including an agent acting beyond the capabilities its scoped identity grants
- Any path that mutates the database without a snapshot, or that makes a snapshot unrestorable
- Governance bypass: any way to widen permissions rather than narrow them, or to suppress the audit log
- Injection of any kind: SQL, command, path traversal, stored or reflected cross-site scripting in the admin screens
- Missing capability checks or nonce checks on admin actions
- A way to reach the guarded WP-CLI or PHP execution paths on a production environment, or with them disabled
- Server-side request forgery through the tools that fetch remote URLs

## What is not in scope

These are documented behaviours, not vulnerabilities:

- The guarded WP-CLI and PHP snippet abilities executing code when an administrator has deliberately enabled them in a development environment. That is what they are for. A way to reach them without both conditions being met **is** in scope.
- An administrator granting an agent broad capabilities and the agent using them. Governance narrows permissions, it does not second-guess the operator.
- Snapshots not covering a plugin's custom database tables where the integration is documented as read-only for exactly that reason.
- Findings that require an already-compromised administrator account or existing filesystem access.
- Issues in third-party plugins, themes, or WordPress core itself. Report those upstream.
- Automated scanner output with no demonstrated impact.

## For operators

- Snapshots are per-operation undo, not backups. Keep a real backup solution.
- Give each agent a scoped identity with only the capabilities it needs.
- Leave the execution abilities off in production. They ship off by default.
- Review the audit log after an unattended agent session.
