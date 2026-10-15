# Woovi PHP SDK

PHP SDK for the Woovi (OpenPix) REST API. Independent fork of
`woovibr/php-sdk` (`open-pix/php-sdk`), detached from its fork network and
published as `ownerpro/woovi-php-sdk` (repo `OwnerPro-Software/woovi-php-sdk`).

## Quality Checks

```bash
composer install
composer test               # PHPUnit 9; also writes html-coverage/
composer stan               # PHPStan level max on src/ and tests/
composer cs:check           # PHP CS Fixer, @PSR12 (.php-cs-fixer.dist.php)
composer cs:fix             # auto-fix style
composer audit
```

CI (`.github/workflows/ci.yml`) runs PHPUnit on PHP 7.3 to 8.5 and the
linters on 8.5. The test jobs remove php-cs-fixer and phpstan before
installing, because php-cs-fixer requires PHP >= 7.4.

`composer.lock` is gitignored (library), so dependency updates never produce a
lock commit. Never disable Composer's `block-insecure`: raise the constraint of
the affected package instead.

## Project

- PSR-4: `OpenPix\PhpSdk\` → `src/`, `Tests\` → `tests/`. The namespace is the
  upstream one, so `composer.json` declares a `conflict` with
  `open-pix/php-sdk`.
- Supports `^7.3 || ^8.0`; keep syntax compatible with PHP 7.3 (no typed
  properties, arrow functions or union types).
- `Client::SDK_VERSION` (`1.1.5`) is sent to the API in the `version` header.
  It is not the package version: leave it unchanged on releases.
- `RequestTransport::hydrateResponse` throws `ApiErrorException` for a
  definitive API rejection (status code and decoded body) and
  `UnreadableResponseException` when the body cannot be read. An unreadable 2xx
  may have been processed, so callers reconcile by `correlationID` instead of
  retrying.

## Releasing

Distributed only through the GitHub VCS repository, not Packagist. The git tag
is the version: `composer.json` has no `version` field. Tags have a `v` prefix
(`v0.1.0`), like the other OwnerPro SDKs; consumers pin `0.1.0`.

1. Add the release section to `CHANGELOG.md` and commit.
2. `git tag -a vX.Y.Z -m "vX.Y.Z"` and `git push origin main vX.Y.Z`.
3. `gh release create vX.Y.Z --verify-tag --title vX.Y.Z --notes-file <section>`,
   with the notes taken from that version's `CHANGELOG.md` section.

Composer takes the package `name` from the default branch (`main`) for every
branch and tag of a VCS repository, so a rename takes effect only when it
reaches `main`, and then applies to every branch at once.

## Branches and Upstream

- `main` is the default branch and the only one that receives commits.
- `pulsar` is the branch the pulsar API consumed as
  `"open-pix/php-sdk": "dev-pulsar"` before `v0.1.0`. Since the rename reached
  `main`, Composer lists it as `ownerpro/woovi-php-sdk dev-pulsar`, so that
  requirement no longer resolves on `composer update`. Old locks still install:
  they pin commit `ce5b43e`, which `main` contains.
- `accounts`, `last-page-bug`, `partners`, `patch-1`, `patch-2`,
  `paymentApproval` and `statement` are heads of old pull requests to the
  upstream. Their changes are already in `main` or in the upstream.
- There is no fork link. To bring in an upstream change:
  `git fetch https://github.com/woovibr/php-sdk.git main`, then cherry-pick.

## Remotes

`origin` fetches from GitHub (primary) and pushes to GitHub and to the Gitea
backup (`gitea@gitea.ownerprobusiness.com.br:OwnerPro-Software/woovi-php-sdk.git`).
`git push origin --all` only sends local branches, so keep a local branch for
every GitHub branch. After a release, `git ls-remote` on both must list the
same refs, ignoring `refs/pull/`.
