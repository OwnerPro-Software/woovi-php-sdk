# Woovi PHP SDK

[![CI](https://github.com/OwnerPro-Software/woovi-php-sdk/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/OwnerPro-Software/woovi-php-sdk/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

This SDK provides convenient access to the Woovi (OpenPix) REST API, allowing you to easily integrate payment services into your PHP applications.

<p align="center">
  <img src="example.png" alt="usage example screenshot" width="700">
</p>

## Documentation

See [SDK documentation](https://developers.openpix.com.br/docs/sdk/php/sdk-php-what-is) and [Rest API documentation](https://developers.openpix.com.br/api).

## Installation

The package is not published on Packagist; install it straight from GitHub with [Composer](https://getcomposer.org/), together with a PSR-18 HTTP client and PSR-17 factories:

```bash
composer config repositories.woovi-php-sdk vcs https://github.com/OwnerPro-Software/woovi-php-sdk
composer require ownerpro/woovi-php-sdk guzzlehttp/guzzle guzzlehttp/psr7
```

It keeps the `OpenPix\PhpSdk` namespace of `open-pix/php-sdk`, so it conflicts with that package rather than installing alongside it.

## Basic usage

Here is the basic usage of the SDK. See [SDK documentation](https://developers.openpix.com.br/docs/sdk/php/sdk-php-usage) for more details.

```php
use OpenPix\PhpSdk\Client;

// Load autoload of Composer.
require_once __DIR__ . "/vendor/autoload.php";

$client = Client::create("YOUR_APP_ID");

// Create a customer.
$customer = [
    "name" => "Dan PHP-SDK",
    "taxID" => "00000000000", // CPF
    "email" => "email0@example.com",
    "phone" => "5511999999999",
    "correlationID" => "test-php-sdk-customer-" . mt_rand(1, 10000),
];

$client->customers()->create($customer);

// Create a charge using above customer.
$charge = [
    // Charge value.
    "value" => 1000, // (R$ 10,00)

    // Your correlation ID to keep track of this charge.
    "correlationID" => "test-php-sdk-charge-" . mt_rand(1, 10000),

    // Charge customer.
    "customer" => $customer,
];

$result = $client->charges()->create($charge);

// Get the generated dynamic BR code to be rendered as a QR Code.
echo $result["brCode"] . "\n";
```

## Fork & Credits

OwnerPro-Software maintains this package as an independent fork of [woovibr/php-sdk](https://github.com/woovibr/php-sdk) (`open-pix/php-sdk`), taken from its `main` after v1.1.4. Changes since the fork are listed in the [CHANGELOG](CHANGELOG.md).

## License

Distributed under the terms of the [MIT license](LICENSE).
