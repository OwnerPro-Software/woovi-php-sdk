# Changelog

## [0.1.0](https://github.com/OwnerPro-Software/woovi-php-sdk/releases/tag/v0.1.0) (2026-10-15)

First release as `ownerpro/woovi-php-sdk`, an independent fork of [woovibr/php-sdk](https://github.com/woovibr/php-sdk) (`open-pix/php-sdk`). Version numbers restart here. The sections below 0.1.0 are the upstream history; its 1.1.5 was never released.

### Features

* `Statements` resource: `$client->statements()->list()` on `/api/v1/statement`.
* `ApiErrorException` carries the HTTP status code (`getStatusCode()`), the decoded response body (`getBody()`) and an optional previous throwable.
* `UnreadableResponseException`, with `getStatusCode()`, is thrown when a response body cannot be decoded or is not an array, instead of a raw `JsonException` or `TypeError`.

### Bug Fixes

* An error status (>= 400) whose body has no `error` key raises `ApiErrorException` instead of being returned as data. Its message falls back to `message`, `description`, `errorMessage` and finally the status and reason phrase.
* The message of an `error` object is read from `message`, then `description`, then the encoded object, without an undefined key error.
* A 204 response returns an empty array.
* `Paginator` no longer fails on a single page nor skips the last page of multi-page results.
* `Paginator` falls back to `hasNextPage` when `pageInfo` has no `totalCount`, as in `/api/v1/transaction`.
* The `$lastResult` parameter of the `Paginator` constructor is explicitly nullable, removing the PHP 8.4 deprecation.

### Miscellaneous

* The package is named `ownerpro/woovi-php-sdk` and conflicts with `open-pix/php-sdk`, whose `OpenPix\PhpSdk` namespace it keeps.
* `Client::SDK_VERSION` stays `1.1.5`, the value sent in the `version` header.
* GitHub Actions run `ci.yml` (tests, PHPStan and PHP CS Fixer) instead of the upstream workflows.


## [1.1.5](https://github.com/Open-Pix/php-sdk/compare/v1.1.4...v1.1.5) (2024-11-26)

### Partners Resource

* Partners Resource. [#58](https://github.com/woovibr/php-sdk/pull/58)
* Accounts Resource. [#62](https://github.com/woovibr/php-sdk/pull/52)
* approve() method added to Payments resource. [#61](https://github.com/woovibr/php-sdk/pull/51)
* php 8.3 and 8.4 added to code quality workflow. [#59](https://github.com/woovibr/php-sdk/pull/59)


## [1.1.4](https://github.com/Open-Pix/php-sdk/compare/v1.1.3...v1.1.4) (2024-11-19)

### Webhook IPs

* Added a method to retrieve the IPs used by Webhooks to send notifications [#56](https://github.com/woovibr/php-sdk/pull/56)


## [1.1.3](https://github.com/Open-Pix/php-sdk/compare/v1.1.2...v1.1.3) (2024-01-04)


### Bug Fixes

* **php:** add support for php 7.3 ([#54](https://github.com/Open-Pix/php-sdk/issues/54)) ([a3fbc66](https://github.com/Open-Pix/php-sdk/commit/a3fbc66c21c9b825e7f2e18a77409e28fff678cb))

## [1.1.2](https://github.com/Open-Pix/php-sdk/compare/v1.1.1...v1.1.2) (2023-10-19)


### Miscellaneous Chores

* **deps:** allow psr/http-message ^1.0 ([#51](https://github.com/Open-Pix/php-sdk/issues/51)) ([54537ad](https://github.com/Open-Pix/php-sdk/commit/54537ad4a62b329397bf98daac6f29910aef4681))

## [1.1.1](https://github.com/Open-Pix/php-sdk/compare/v1.1.0...v1.1.1) (2023-06-26)


### Miscellaneous Chores

* **actions:** add spelling CI job ([#41](https://github.com/Open-Pix/php-sdk/issues/41)) ([e661bb2](https://github.com/Open-Pix/php-sdk/commit/e661bb2afc2213a73637316477284720c2e1af49))

## [1.1.0](https://github.com/Open-Pix/php-sdk/compare/v1.0.0...v1.1.0) (2023-06-23)


### Features

* **webhooks:** add webhook validation method ([d0a5c9c](https://github.com/Open-Pix/php-sdk/commit/d0a5c9c1f2137c953296925e8e705e234f31ea9a))


### Miscellaneous Chores

* **actions:** add release workflow ([#37](https://github.com/Open-Pix/php-sdk/issues/37)) ([138420b](https://github.com/Open-Pix/php-sdk/commit/138420bcaf018f7f318b7749e9efae92ffd9cc21))
