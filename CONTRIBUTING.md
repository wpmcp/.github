# Contributing

These are the org-wide defaults for repositories under [wpmcp](https://github.com/wpmcp). Most contributions belong in [wpmcp/wpmcp](https://github.com/wpmcp/wpmcp), the plugin itself. A repo with its own CONTRIBUTING.md overrides this file.

Issues and pull requests are welcome. For anything larger than a bug fix, open an issue first so the approach can be agreed before you spend time on it.

## The one rule that is not negotiable

No tool may write to the database except through the safety layer. Every mutating path takes a snapshot first so the operation can be restored. A pull request that adds a write bypassing `Safe_Mutation::run()` will not be merged, however useful the feature is. The recoverability guarantee is the product.

Related invariants:

- Governance layers may only narrow permissions, never widen them.
- Anything reachable from an agent is capability-checked and its input validated.
- There is exactly one `eval()` call site in the plugin, inside the guarded PHP snippet runner. Do not add another.
- Execution paths (WP-CLI, PHP snippets) stay disabled by default and restricted to development environments.

## Local setup

Requires PHP 8.1 or newer and a MySQL or MariaDB database for the WordPress integration harness.

```bash
composer install
bash bin/install-wp-tests.sh wordpress_test root root 127.0.0.1 6.9
bash bin/install-test-plugins.sh   # Elementor, WooCommerce, ACF, Yoast SEO, Polylang
composer test
composer lint
```

CI runs the suite on PHP 8.1, 8.2, 8.3, and 8.4 against WordPress 6.9, plus a coverage run with a floor that only ever moves up. See `.github/workflows/ci.yml` for the exact setup.

## How work gets done

1. **Write the failing test first.** Free-tier tests live in `tests/free`, paid-tier tests in `tests/pro`. Features usually ship a pair.
2. **Keep the change small.** One capability or one fix per pull request. Ability-adding pull requests all touch the same manifest file, so large branches conflict and stall behind each other.
3. **Regenerate the ability manifest** when you add, remove, rename, or re-tier an ability: `composer manifest:regenerate`, then commit the diff. Run the *full* suite afterwards. A filtered run skips the count assertions and will pass on a broken manifest.
4. **Run the linter.** PSR-12 with WordPress-idiomatic naming, via `composer lint`.
5. **Say what you did not do.** Deferred scope, production-only verification, and known gaps go in the pull request description and the release notes. Honest scope is how this project is positioned, so an accurate limitation is worth more than an optimistic claim.

## Adding an integration

Integrations are dispatchers against the target plugin's real API, with faithful stubs in `tests/support` so the suite can exercise them without the plugin installed.

Before building a dedicated adapter, check whether the generic tools already cover it. Block suites and Elementor addon packs are typically reachable through the existing Gutenberg block tools and the widget tools' raw-settings path. When that is the case, the correct contribution is a regression test proving the coverage, not a new adapter.

Writes need data that can be snapshotted and restored. Plugins that keep their data in custom tables are usually read-only for now, and that is documented rather than worked around.

## Style

- Sentence case headings. Concrete, specific wording.
- **No em-dashes and no en-dashes**, anywhere: code, comments, docs, commit messages, release notes. Use a comma, a colon, a period, or a plain hyphen. This is checked.
- Commit messages are plain and imperative, with a conventional prefix where the repo already uses one (`feat:`, `fix:`, `ci:`).
- Do not name competitor products in code, docs, or public copy.

## Security

Do not open a public issue for a vulnerability. See [SECURITY.md](SECURITY.md).

## License

Contributions to the plugin are licensed under GPL-2.0-or-later, matching the project.
